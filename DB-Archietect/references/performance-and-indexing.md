# Performance & Indexing Guide

Query optimisation, index strategy, N+1 detection, connection pooling, and common performance pitfalls. Load this when spotting performance issues in code or when the schema design phase needs a performance review.

---

## Index Strategy

### When to Add an Index

**Always index:**
- Every foreign key column (`user_id`, `org_id`, `post_id`, etc.)
- Columns used in `WHERE` clauses on large tables
- Columns used in `ORDER BY` on frequently-queried result sets
- `UNIQUE` constraints (auto-creates an index)
- Columns used in `JOIN` conditions

**Consider indexing:**
- Columns with high cardinality (many distinct values) — emails, UUIDs, slugs
- Composite indexes for multi-column `WHERE` clauses (column order matters — most selective first)
- Partial indexes for filtered queries (e.g. only active records)

**Avoid indexing:**
- Low-cardinality boolean/status columns on their own (e.g. `is_active` alone is usually not worth it)
- Tables under ~10k rows (full scan is fine)
- Columns only used in `SELECT`, never `WHERE`/`JOIN`/`ORDER`

### Index Types (PostgreSQL)

```sql
-- Standard B-tree (default, covers most cases)
CREATE INDEX idx_posts_user ON posts(user_id);

-- Composite (column order = selectivity order)
CREATE INDEX idx_posts_status_date ON posts(status, published_at DESC);

-- Partial (only index rows matching a condition)
CREATE INDEX idx_users_unverified ON users(email) WHERE email_verified_at IS NULL;
CREATE INDEX idx_orders_pending ON orders(created_at) WHERE status = 'pending';

-- GIN (full-text search, JSONB, arrays)
CREATE INDEX idx_posts_search ON posts USING GIN(search_vector);
CREATE INDEX idx_products_meta ON products USING GIN(metadata);

-- BRIN (very large tables with natural ordering, e.g. append-only logs)
CREATE INDEX idx_logs_created ON activity_log USING BRIN(created_at);

-- Covering index (include columns to avoid table heap fetch)
CREATE INDEX idx_posts_slug ON posts(slug) INCLUDE (title, status, published_at);
```

### EXPLAIN ANALYZE — Read the Query Plan

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM posts WHERE author_id = $1 AND status = 'published' ORDER BY published_at DESC LIMIT 20;
```

**What to look for:**
- `Seq Scan` on large tables → missing index
- `Index Scan` → good, using index
- `Bitmap Heap Scan` → index used but lots of rows, might need partial index
- High `rows=` estimate mismatch → run `ANALYZE tablename` to refresh stats
- `Hash Join` vs `Nested Loop` → Nested Loop is better when inner side is small

---

## N+1 Query Detection

The N+1 problem: fetching 1 list then making N individual queries for related data.

### Spotting N+1 in Code

**Classic Node.js/Express N+1 pattern:**
```javascript
// BAD — N+1: 1 query for posts + N queries for each author
const posts = await db.query('SELECT * FROM posts WHERE status = $1', ['published'])
for (const post of posts) {
  post.author = await db.query('SELECT * FROM users WHERE id = $1', [post.author_id]) // N queries!
}

// GOOD — JOIN in single query
const posts = await db.query(`
  SELECT p.*, u.name AS author_name, u.avatar_url
  FROM posts p JOIN users u ON u.id = p.author_id
  WHERE p.status = $1
`, ['published'])
```

**Prisma N+1 pattern:**
```typescript
// BAD — N+1
const posts = await prisma.post.findMany()
for (const post of posts) {
  const author = await prisma.user.findUnique({ where: { id: post.authorId } }) // N queries!
}

// GOOD — include (single JOIN query)
const posts = await prisma.post.findMany({
  include: { author: { select: { name: true, avatarUrl: true } } }
})
```

**Mongoose N+1:**
```javascript
// BAD
const posts = await Post.find({ status: 'published' })
for (const post of posts) {
  post._author = await User.findById(post.authorId) // N queries!
}

// GOOD — populate
const posts = await Post.find({ status: 'published' })
  .populate('authorId', 'name avatarUrl')

// EVEN BETTER for MongoDB — embed when data always read together
// Store author name + avatar directly in the post document
```

### Flag These Patterns in Code Review
- Any DB query inside a `for`/`forEach`/`map` loop
- Multiple `findById` calls in a row that could be `findMany` + map
- Route handlers that make > 3 sequential DB calls without batching

---

## Connection Pooling

### Why It Matters

Opening a new DB connection per request is expensive (~50-100ms). Connection pools keep N connections open and reuse them.

### PostgreSQL Pool Settings

```javascript
// node-postgres (pg)
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20,                    // max connections in pool
  idleTimeoutMillis: 30000,   // close idle connections after 30s
  connectionTimeoutMillis: 2000, // fail fast if can't get connection
  ssl: process.env.NODE_ENV === 'production' ? { rejectUnauthorized: false } : false
})
```

**Pool sizing rule of thumb:**
- Development: `max: 5`
- Production (single server): `max: 20`
- Serverless (Vercel/Netlify): Use PgBouncer or Supabase's pooled connection URL — don't set high max, serverless functions can spawn hundreds of instances

### Serverless Pooling (Critical)

Serverless functions create a new process per invocation. Without pooling middleware, you'll exhaust Postgres's `max_connections` fast.

**Solutions:**
```
Supabase → Use the "Transaction" pooler URL (port 6543, not 5432)
Neon → Built-in serverless driver handles this
PlanetScale → Built-in serverless connection pooling
Self-hosted Postgres → Deploy PgBouncer in transaction mode
```

**Prisma + Supabase pooled connection:**
```env
# .env
DATABASE_URL="postgresql://USER:PASS@HOST:6543/DB?pgbouncer=true"  # pooled (transactions)
DIRECT_URL="postgresql://USER:PASS@HOST:5432/DB"                    # direct (migrations)
```

```prisma
datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}
```

---

## Query Optimisation Patterns

### Pagination: Cursor vs Offset

```sql
-- OFFSET pagination (slow for large offsets — scans + discards rows)
SELECT * FROM posts ORDER BY created_at DESC LIMIT 20 OFFSET 2000; -- slow!

-- CURSOR pagination (fast — starts from known position)
SELECT * FROM posts
WHERE created_at < $1  -- cursor = last item's created_at from previous page
ORDER BY created_at DESC
LIMIT 20;
```

Use cursor pagination for any table > 10k rows or infinite scroll UI.

### Avoid SELECT *

```sql
-- BAD — fetches all columns including large text/blob fields
SELECT * FROM posts WHERE status = 'published';

-- GOOD — fetch only what the UI needs
SELECT id, title, slug, excerpt, published_at FROM posts WHERE status = 'published';
```

In Prisma, always use `select`:
```typescript
const posts = await prisma.post.findMany({
  select: { id: true, title: true, slug: true, publishedAt: true }
})
```

### Batch Inserts Over Individual Inserts

```typescript
// BAD — N round trips
for (const item of items) {
  await prisma.orderItem.create({ data: item })
}

// GOOD — single round trip
await prisma.orderItem.createMany({ data: items })
```

### Read Replicas for Heavy Read Workloads

```typescript
// Prisma read replica setup
const prisma = new PrismaClient({
  datasources: {
    db: { url: process.env.DATABASE_URL }, // primary (writes)
  },
})

// Route reads to replica
const readPrisma = new PrismaClient({
  datasources: { db: { url: process.env.READ_REPLICA_URL } }
})
```

---

## MongoDB Performance

### Index on Queried Fields
```javascript
// Always index fields used in find() queries
userSchema.index({ email: 1 })
postSchema.index({ authorId: 1, status: 1, publishedAt: -1 }) // compound for common query
```

### Avoid Unbounded Arrays
```javascript
// BAD — array can grow to millions of items
{ userId: '...', tags: ['tag1', 'tag2', ... potentially millions] }

// GOOD — cap at reasonable size or use a separate collection
{ userId: '...', tags: ['tag1', 'tag2'] } // max ~100 tags
// For large associations, use a separate collection with references
```

### Projection (select only needed fields)
```javascript
// BAD
const posts = await Post.find({ status: 'published' }) // fetches content field too (can be MB)

// GOOD
const posts = await Post.find({ status: 'published' }, 'title slug excerpt publishedAt -_id')
```

### Use Explain
```javascript
await Post.find({ status: 'published' }).sort({ publishedAt: -1 }).limit(20).explain('executionStats')
// Look for: IXSCAN (good) vs COLLSCAN (bad — missing index)
```

---

## Common Mistakes to Flag in Code

| Pattern Spotted | Problem | Fix |
|---|---|---|
| DB query in a loop | N+1 | Batch with `findMany` + map, or JOIN |
| `SELECT *` on large table | Over-fetching | Select only needed columns |
| No index on FK column | Slow JOINs | Add index on FK |
| Offset pagination > 1000 | Full table scan | Switch to cursor pagination |
| No connection pooling | Connection exhaustion | Add Pool or use pooler URL |
| Missing `LIMIT` on user-facing queries | Unbounded result sets | Always paginate |
| Synchronous ORM calls in async handler | Blocks event loop | Always `await` async DB calls |
| Storing binary data in DB | Bloated DB | Use object storage (S3/Supabase Storage) |
| Fetching in `useEffect` without caching | Repeated DB hits | Cache with SWR/React Query |
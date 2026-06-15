# No-Migration Alternatives Reference

What to do instead when migration is the wrong call. This file is loaded when the Evolution Engine returns a NO-GO or PARTIAL GO verdict, or when the user asks "what else can we do?"

The core principle: **most migration goals can be achieved within the current stack for a fraction of the cost.** Migration is almost never the only path.

---

## Table of Contents

1. [Before You Conclude NO-GO](#before-you-conclude)
2. [Performance Problems](#performance-problems)
3. [Maintainability Problems](#maintainability-problems)
4. [Scalability Problems](#scalability-problems)
5. [Developer Experience Problems](#dx-problems)
6. [Database Problems](#database-problems)
7. [Security Problems](#security-problems)
8. [Cost Problems](#cost-problems)
9. [The Modular Monolith](#modular-monolith)
10. [Incremental Modernisation Checklist](#incremental-modernisation)

---

## Before You Conclude NO-GO

Run this diagnostic before recommending alternatives. Identify the actual problem, not the stated solution:

```
The user says: "We need to migrate to X"
The real question: "What problem is X supposed to solve?"

Common mismatches:
  "Migrate to Go"          → Real problem: slow Python endpoints
  "Migrate to Postgres"    → Real problem: Mongo queries are hard to write
  "Migrate to K8s"         → Real problem: deployment is manual and fragile
  "Migrate to microservices" → Real problem: two teams blocking each other on releases
  "Rewrite in TypeScript"  → Real problem: runtime errors in production
  "Migrate to GraphQL"     → Real problem: mobile app over-fetches data
```

Always diagnose the root cause. The alternatives below are organised by the **problem**, not by the proposed migration.

---

## Performance Problems

### "Our app is too slow — we should rewrite in Go / Rust"

Before a language migration, exhaust these options:

**1. Profile first**
```bash
# Node.js
node --prof app.js
node --prof-process isolate-*.log

# Python
python -m cProfile -o output.prof app.py
snakeviz output.prof

# Java
async-profiler / JFR
```
In most cases, 80% of latency is in 5% of the code. Fix those 5% first.

**2. Fix N+1 queries (most common culprit)**
```typescript
// Before: N+1 — 1 query for users, N queries for posts
const users = await User.findAll();
for (const user of users) {
  user.posts = await Post.findAll({ where: { userId: user.id } });
}

// After: 2 queries total
const users = await User.findAll({ include: [Post] });
```

**3. Add database indexes**
```sql
-- Find slow queries
EXPLAIN ANALYSE SELECT * FROM orders WHERE user_id = 123 AND status = 'pending';

-- Add the missing index
CREATE INDEX CONCURRENTLY idx_orders_user_status ON orders(user_id, status);
```
A missing index on a hot query path can cause 100–1000× slowdown. This is almost always cheaper to fix than a migration.

**4. Add caching**
```typescript
// Redis cache layer — add in hours, not weeks
const cached = await redis.get(`user:${id}`);
if (cached) return JSON.parse(cached);

const user = await db.users.findById(id);
await redis.setex(`user:${id}`, 300, JSON.stringify(user));
return user;
```

**5. Move CPU-bound work off the main thread**
```typescript
// Node.js: offload to worker thread
import { Worker } from 'worker_threads';
const result = await runInWorker('./heavy-compute.js', data);

// Python: offload to Celery
@celery.task
def heavy_compute(data):
    return process(data)
```

**6. Add a read replica**
Route read queries to a replica, writes to primary. Typically doubles read capacity for the cost of one extra DB instance — no code migration required.

**When language migration IS justified:**
- You have profiled, and the bottleneck is genuinely the language runtime (rare)
- The workload is CPU-bound and memory-intensive (ML inference, video processing, compression)
- You need sub-millisecond latency that GC pauses prevent

---

## Maintainability Problems

### "The codebase is a mess — we should rewrite it"

A rewrite doesn't delete technical debt. It moves it. The new system will accumulate the same debt if the practices don't change.

**Alternative 1: Targeted Refactoring**

Identify the highest-pain files (most bugs, most change frequency, most developer complaints). Refactor those specifically:

```bash
# Find files changed most often (high churn = high pain)
git log --name-only --pretty=format: | sort | uniq -c | sort -rn | head -20

# Find files with most bug-fix commits
git log --oneline --grep="fix" --name-only | grep "\.ts$" | sort | uniq -c | sort -rn
```

Refactor the top 10 files. This fixes 80% of the pain for 10% of the rewrite cost.

**Alternative 2: Add Types Incrementally (JS → TS without full migration)**

```json
// tsconfig.json — enable type checking on JS files without renaming them
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
    "strict": false
  }
}
```

Then add JSDoc types to the most-used functions:
```javascript
/**
 * @param {string} userId
 * @returns {Promise<User>}
 */
async function getUser(userId) { ... }
```

This gives you 70% of TypeScript's safety with 10% of the migration effort.

**Alternative 3: Establish Module Boundaries Without Microservices**

```
src/
  modules/
    users/          ← owns everything user-related
      index.ts      ← public API (only import this)
      internal/     ← private to this module
    orders/
      index.ts
      internal/
    payments/
      index.ts
      internal/
```

Enforce with ESLint:
```json
// .eslintrc — no-restricted-imports to enforce boundaries
{
  "rules": {
    "no-restricted-imports": ["error", {
      "patterns": ["*/modules/*/internal/*"]
    }]
  }
}
```

---

## Scalability Problems

### "We need to migrate to microservices to scale"

Microservices solve organisational scale (many teams, many independent deployments). They don't automatically solve traffic scale. Try these first:

**1. Horizontal scaling of the monolith**
```yaml
# docker-compose.yml — scale the existing app to N replicas
services:
  app:
    image: your-app
    deploy:
      replicas: 5
```
Most monoliths can scale to handle 10–100× traffic by adding replicas behind a load balancer. This requires the app to be stateless (externalise sessions to Redis).

**2. Extract only the hot path**
Instead of full microservices, extract the single highest-load endpoint into its own service:
```
Before: monolith handles everything

After:
  /api/search → dedicated search service (can scale independently)
  everything else → monolith (unchanged)
```
This is a Strangler Fig applied to one seam, not the whole system.

**3. Queue heavy work**
```typescript
// Move slow operations out of the request path
app.post('/api/export', async (req, res) => {
  const jobId = await queue.add('export', { userId: req.user.id, params: req.body });
  res.json({ jobId, status: 'queued' });  // respond immediately
});

// Worker processes export asynchronously
queue.process('export', async (job) => {
  const result = await generateExport(job.data);
  await notify(job.data.userId, result);
});
```

**When microservices ARE justified:**
- >50 engineers are blocked on each other for deployments
- Different services genuinely need different scaling characteristics
- You have a platform team to own the infrastructure

---

## Developer Experience Problems

### "DX is terrible — we should switch frameworks"

**1. Add a local dev script that actually works**
```bash
# scripts/dev.sh — one command to start everything
#!/bin/bash
docker-compose up -d postgres redis
npm run db:migrate
npm run dev
```

**2. Improve error messages**
```typescript
// Add context to errors instead of letting them bubble as generic 500s
try {
  await processOrder(orderId);
} catch (err) {
  throw new AppError(`Failed to process order ${orderId}`, {
    cause: err,
    context: { orderId, userId },
    code: 'ORDER_PROCESSING_FAILED'
  });
}
```

**3. Add hot reload and better tooling**
```bash
# Node.js: switch from nodemon to tsx watch (faster, TS-native)
npx tsx watch src/index.ts

# Python: switch to uvicorn with --reload
uvicorn main:app --reload

# Go: use air for hot reload
air
```

**4. Add a proper logging setup**
```typescript
// Structured logging makes debugging 10× faster
import pino from 'pino';
const log = pino({ level: process.env.LOG_LEVEL || 'info' });

log.info({ userId, action: 'login' }, 'User logged in');
// Output: {"level":30,"userId":"123","action":"login","msg":"User logged in"}
```

---

## Database Problems

### "MongoDB is painful — we should migrate to PostgreSQL"

Before migrating the database (highest-risk migration type), try:

**1. Fix query patterns**
```javascript
// Bad: loading entire document to read one field
const user = await User.findById(id);
const name = user.profile.name;

// Better: project only needed fields
const user = await User.findById(id, 'profile.name');
```

**2. Add the right indexes**
```javascript
// Add compound index for common query patterns
db.orders.createIndex({ userId: 1, createdAt: -1 });
db.orders.createIndex({ status: 1, userId: 1 });
```

**3. Use aggregation pipelines instead of application-level joins**
```javascript
// Instead of fetching users then fetching orders for each:
const result = await User.aggregate([
  { $match: { _id: userId } },
  { $lookup: {
      from: 'orders',
      localField: '_id',
      foreignField: 'userId',
      as: 'orders'
  }},
  { $project: { name: 1, email: 1, orderCount: { $size: '$orders' } } }
]);
```

**4. Use JSONB in PostgreSQL if you're already there**
If you have a partially dynamic schema, add a `metadata JSONB` column instead of migrating to a document store:
```sql
ALTER TABLE users ADD COLUMN metadata JSONB DEFAULT '{}';
CREATE INDEX idx_users_metadata ON users USING gin(metadata);

-- Query JSON fields efficiently
SELECT * FROM users WHERE metadata->>'plan' = 'pro';
```

**When DB migration IS justified:**
- You need ACID transactions across multiple documents (MongoDB has them but they're painful)
- Your data is genuinely relational and you're fighting Mongo's document model
- You need complex reporting queries that MongoDB aggregation can't express cleanly

---

## Security Problems

### "We need to rewrite to fix our security posture"

Security improvements are almost always additive — they don't require a migration.

**1. Add security headers (30 minutes)**
```typescript
import helmet from 'helmet';
app.use(helmet());
// Adds: CSP, HSTS, X-Frame-Options, X-Content-Type-Options, etc.
```

**2. Fix input validation (1–2 days)**
```typescript
import { z } from 'zod';

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  role: z.enum(['user', 'admin'])
});

app.post('/users', (req, res) => {
  const data = CreateUserSchema.parse(req.body);  // throws on invalid input
  // ...
});
```

**3. Audit dependencies (2–4 hours)**
```bash
npm audit --audit-level=high
pip-audit
cargo audit
```
Fix high/critical CVEs by updating packages. This resolves most security audit findings without any migration.

**4. Add rate limiting (2 hours)**
```typescript
import rateLimit from 'express-rate-limit';
app.use('/api/', rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }));
```

---

## Cost Problems

### "Our AWS / GCP bill is too high — we should migrate platforms"

Cloud cost optimisation is almost always faster than platform migration.

**1. Right-size instances**
```bash
# AWS: enable Compute Optimizer and follow recommendations
# Typically finds 20–40% savings in 1 week of analysis
```

**2. Add autoscaling**
```yaml
# Scale down at night, up during business hours
aws autoscaling put-scaling-policy \
  --policy-type TargetTrackingScaling \
  --target-tracking-configuration '{"TargetValue": 60, "PredefinedMetricSpecification": {"PredefinedMetricType": "ASGAverageCPUUtilization"}}'
```

**3. Use spot / preemptible instances for non-critical workloads**
```yaml
# Background workers on spot = 60–80% cheaper
nodeGroups:
  - name: workers
    instancesDistribution:
      onDemandPercentageAboveBaseCapacity: 20
      spotInstancePools: 3
```

**4. Move to a cheaper region**
```
us-east-1 → us-east-2: ~5% cheaper
eu-west-1 → eu-central-1: ~3% cheaper
```

**When platform migration IS justified:**
- You've exhausted right-sizing and reserved instances
- A vendor is removing a service you depend on
- Regulatory requirements mandate a specific provider

---

## The Modular Monolith

The most underrated architecture. A well-structured monolith outperforms microservices for teams under ~50 engineers.

### What It Is

A single deployable unit with strict internal module boundaries:

```
src/
  modules/
    users/
      users.controller.ts    ← HTTP handlers
      users.service.ts       ← business logic
      users.repository.ts    ← data access
      users.types.ts         ← types / DTOs
      index.ts               ← public API (only this file is importable externally)
    orders/
      ...
      index.ts
    payments/
      ...
      index.ts
  shared/
    database/
    logging/
    config/
  app.ts
```

### What You Get

- Independent module development (teams don't step on each other)
- Independent testing (test each module in isolation)
- Easy extraction to microservices later if truly needed (modules are already decoupled)
- Single deployment, single repo, single observability setup
- No network calls between modules (no distributed systems problems)

### Enforcement

```typescript
// Each module exports a clean public API
// users/index.ts
export { UsersService } from './users.service';
export type { User, CreateUserDto } from './users.types';
// Note: UsersRepository is NOT exported — it's internal

// ESLint rule prevents importing internal module files
// "no-restricted-imports" blocks: "*/modules/*/users.repository"
```

---

## Incremental Modernisation Checklist

When the verdict is PARTIAL GO or NO-GO, offer this checklist as an alternative roadmap:

```yaml
Incremental Modernisation Roadmap:
  
  Week 1–2: Observability (zero risk, high value)
    - [ ] Add structured logging (pino, winston, zerolog)
    - [ ] Add distributed tracing (OpenTelemetry)
    - [ ] Add error tracking (Sentry)
    - [ ] Add performance monitoring (Datadog, New Relic, or Prometheus)

  Week 3–4: Security Hardening (low risk)
    - [ ] Security headers (helmet)
    - [ ] Input validation on all endpoints (zod, joi, pydantic)
    - [ ] Dependency audit and updates
    - [ ] Rate limiting on public endpoints

  Week 5–8: Performance (low–medium risk)
    - [ ] Identify and fix top 5 N+1 query patterns
    - [ ] Add indexes to top 10 slow queries
    - [ ] Add Redis caching for frequently-read, infrequently-changed data
    - [ ] Enable compression middleware

  Week 9–12: Developer Experience (low risk)
    - [ ] Add TypeScript (allowJs mode — no file renames required)
    - [ ] Standardise error handling pattern
    - [ ] Add hot reload to dev environment
    - [ ] Improve local dev setup (one-command startup)

  Month 4–6: Architecture (medium risk, highest value)
    - [ ] Introduce module boundaries (no-restricted-imports)
    - [ ] Separate business logic from HTTP layer
    - [ ] Add repository pattern for data access
    - [ ] Add integration test suite against the current system

  Outcome: A production-grade, maintainable system without the risk
  or cost of a full migration.
```

# Database Comparison Deep Dive

Load this file when the user needs detailed tradeoff analysis, or when the decision between two databases is close and needs to be argued with specifics.

---

## PostgreSQL

**Best for:** Relational data with clear entity relationships, financial applications, SaaS products, anything requiring ACID compliance, complex queries, and strong consistency.

### Strengths
- Full ACID compliance — transactions are rock solid
- Rich SQL — window functions, CTEs, lateral joins, recursive queries
- JSONB support — can handle semi-structured data alongside relational
- Mature ORM ecosystem: Prisma, Drizzle, TypeORM, SQLAlchemy, ActiveRecord, GORM
- Row-level security — native multi-tenant isolation
- Full-text search built in (tsvector/tsquery)
- PostGIS for geospatial queries
- Excellent managed options everywhere

### Weaknesses
- Schema migrations require planning — breaking changes need care
- Horizontal write scaling is harder (read replicas help, writes are single-node)
- Verbose for truly document-oriented data

### When NOT to use
- You need sub-10ms real-time sync across clients
- Your data has genuinely variable/schemaless shape per record
- You're building mobile-first and already in the Firebase ecosystem

### Hosted Options & Pricing (2024)
| Provider | Free Tier | Paid From |
|---|---|---|
| Supabase | 500MB, pauses after 1 week inactive | $25/mo Pro |
| Neon | 3GB, autoscales to zero | $19/mo Launch |
| Railway | $5 credit/mo | $5/mo |
| Render | 90-day free trial | $7/mo |
| AWS RDS | 12 months free (t3.micro) | ~$15/mo |
| Aiven | 1 free pg instance | ~$19/mo |

---

## MongoDB Atlas

**Best for:** Content platforms, product catalogs, apps with nested/hierarchical data, projects where the schema needs to flex per record, rapid prototyping without migration overhead.

### Strengths
- Schema flexibility — add fields without migrations
- Natural fit for JSON-shaped API data
- Aggregation pipeline — powerful for analytics and transforms
- Atlas Search (Lucene) — full-text search native
- Atlas Vector Search — embeddings/AI workloads built in
- Easy horizontal sharding at scale
- Mongoose is beginner-friendly for JS developers

### Weaknesses
- No joins — `$lookup` works but is expensive at scale
- ACID transactions added later and less battle-tested than Postgres
- Easy to create performance problems with poor document design (unbounded arrays, etc.)
- Free tier is shared, limited to 512MB

### When NOT to use
- Your data has many FK relationships and you keep needing `$lookup`
- You need strong transactional guarantees across multiple collections
- The team is already comfortable with SQL and there's no clear doc-store benefit

### Hosted Options & Pricing
| Tier | Specs | Price |
|---|---|---|
| M0 Free | 512MB, shared | Free |
| M10 | 2GB RAM, dedicated | ~$57/mo |
| Serverless | Pay-per-read/write | Variable, good for MVP |
| Atlas on AWS/GCP/Azure | Full managed | From $57/mo |

---

## Firebase Firestore

**Best for:** Mobile-first apps, real-time collaborative features, apps already using Firebase Auth/Storage, teams wanting a complete BaaS with no server.

### Strengths
- Real-time listeners — live sync across clients out of the box
- Offline support built into mobile SDKs (iOS, Android, Flutter)
- Tight integration with Firebase Auth, Storage, Functions, Hosting
- Query directly from client with security rules — no backend needed
- Autoscales with zero ops
- Generous free tier (Spark plan)

### Weaknesses
- Query limitations — no full-text search, limited compound filters, no aggregations natively
- Expensive at scale — reads and writes are individually billed
- Data modelling is hard — requires denormalisation for performance
- Vendor lock-in (Google)
- No SQL — learning curve for relational thinkers

### When NOT to use
- Complex relational queries across many entity types
- Cost-sensitive high-read/write workloads at scale
- You want to self-host or avoid GCP dependency

### Pricing
| Plan | Limits | Cost |
|---|---|---|
| Spark (free) | 1GB storage, 50k reads/day, 20k writes/day | Free |
| Blaze (pay-as-you-go) | Unlimited | $0.06/100k reads, $0.18/100k writes |

---

## Supabase

**Best for:** Teams that want Postgres + auth + storage + realtime in one product. Excellent DX for Next.js, React, Flutter, and mobile apps.

### Strengths
- Full PostgreSQL underneath — every Postgres feature works
- Auth out of the box: email/password, OAuth (Google, GitHub, etc.), magic link, phone OTP
- Storage built in (S3-compatible bucket API)
- Realtime via Postgres logical replication (table-level subscriptions)
- Row-level security for multi-tenancy
- Auto-generated REST and GraphQL APIs from your schema
- Dashboard is excellent — SQL editor, table UI, auth manager, storage browser
- Edge Functions (Deno-based)

### Weaknesses
- Free tier pauses project after 1 week inactive (requires manual resume or paid plan)
- Realtime is less mature than Firebase for mobile offline
- Some enterprise features are newer/less battle-tested

### When NOT to use
- You need Firebase-grade offline mobile support
- You're deep in AWS and want native RDS with IAM auth
- Your team has strong existing Postgres ops experience and wants full control

### Pricing
| Plan | DB | Storage | Price |
|---|---|---|---|
| Free | 500MB | 1GB | Free (pauses inactive) |
| Pro | 8GB | 100GB | $25/mo |
| Team | Unlimited | Unlimited | $599/mo |

---

## Neon (Serverless Postgres)

**Best for:** Serverless architectures (Vercel, Netlify, Cloudflare), Next.js apps, projects that need branching databases for preview deployments.

### Strengths
- Serverless Postgres — scales to zero when not in use
- Database branching (like Git branches for your DB — perfect for preview URLs)
- Excellent Vercel integration
- Pay-per-compute model — can be very cheap for low-traffic apps
- Full Postgres compatibility

### Weaknesses
- Cold start latency (scales to zero = ~500ms first query)
- Newer product — less mature than RDS or Supabase
- Not ideal for high-frequency persistent connections

### Pricing
- Free: 3GB storage, 1 compute (autoscales to zero)
- Launch: $19/mo — 10GB storage, more compute hours

---

## PlanetScale (Serverless MySQL)

**Best for:** Serverless architectures that need MySQL, zero-downtime schema migrations, teams with Vitess/MySQL experience.

### Strengths
- Database branching — schema changes like Git PRs, no downtime migrations
- Vitess-based horizontal scaling
- Built-in connection pooling for serverless
- Zero-downtime schema changes via deploy requests

### Weaknesses
- No foreign key constraints enforced (by design — Vitess limitation)
- MySQL dialect, not Postgres
- Hobby plan discontinued in 2024 — minimum $39/mo

### When NOT to use
- You need FK constraints enforced at DB level
- Budget-sensitive MVP (minimum $39/mo now)
- Team prefers Postgres ecosystem

---

## Redis

**Best for:** Caching, session storage, rate limiting, pub/sub messaging, job queues, leaderboards. Almost always used **alongside** a primary database.

### Common patterns with Redis
- Session store (express-session, next-auth)
- API response cache (cache expensive queries for 60s)
- Rate limiting (sliding window counter per IP/user)
- Job queue (BullMQ, Celery)
- Pub/sub (real-time events without Firestore cost)
- Leaderboards (sorted sets)

### Hosted Options
| Provider | Free | Paid |
|---|---|---|
| Upstash | 10k commands/day | $0.2/100k commands |
| Redis Cloud | 30MB free | From $7/mo |
| Railway | Included in plan | — |

**Never use as primary persistent store** for business data unless you have AOF persistence configured and understand the tradeoffs.

---

## TimescaleDB

**Best for:** Time-series data, IoT sensor data, application metrics, financial tick data. Runs as a Postgres extension.

### Strengths
- All Postgres features + time-series superpowers
- Automatic partitioning by time (hypertables)
- Compression up to 90% for time-series
- Continuous aggregations
- Works with existing Postgres ORMs

### When to use
- Storing events/logs/metrics with timestamps as primary query dimension
- IoT sensor readings
- Financial OHLCV data
- Any "give me last N hours/days" style queries

---

## Decision Cheat Sheet

```
Has complex relationships (users → orders → items → reviews)?  → PostgreSQL
Building a mobile app with offline support?                    → Firebase
Want everything (auth + DB + storage) in one?                  → Supabase
Flexible schema, nested docs, content platform?                → MongoDB Atlas
Serverless + Next.js + preview branches?                       → Neon
Real-time collaborative (cursors, chat, presence)?             → Firebase or Supabase Realtime
Already on Firebase Auth?                                      → Firestore (stay in ecosystem)
Time-series / IoT data?                                        → TimescaleDB
Analytics / OLAP?                                              → ClickHouse or BigQuery
Caching / sessions on top of any DB?                           → Redis (Upstash)
Zero-downtime MySQL migrations?                                → PlanetScale
```
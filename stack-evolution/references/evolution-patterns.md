# Evolution Patterns Reference

Detailed guidance on migration strategies, when to use each, and how to execute them.

---

## Table of Contents

1. [Strangler Fig Pattern](#strangler-fig)
2. [Big Bang Rewrite](#big-bang)
3. [Parallel Run](#parallel-run)
4. [Branch by Abstraction](#branch-by-abstraction)
5. [Layer-by-Layer Migration](#layer-by-layer)
6. [Database-First Migration](#database-first)
7. [Traffic Splitting Strategy](#traffic-splitting)
8. [Feature Flag Rollouts](#feature-flag-rollouts)
9. [Anti-Patterns to Avoid](#anti-patterns)

---

## Strangler Fig Pattern

**Best for:** Large monoliths → microservices, framework swaps, API migrations
**Risk:** Low (incremental)
**Duration:** Weeks to months

### How It Works

Named after the strangler fig tree that grows around a host tree and eventually replaces it. You build the new system alongside the old, incrementally routing traffic from old → new. When 100% of traffic is on the new system, you retire the old.

```
Initial State:
  All Traffic → Old System

Mid-Migration:
  80% Traffic → Old System
  20% Traffic → New System (new endpoints)

Final State:
  All Traffic → New System
  Old System decommissioned
```

### Implementation Steps

1. **Facade layer** — Add a routing proxy (nginx, API gateway, or code-level router) in front of both systems
2. **Identify seams** — Find natural boundaries to migrate first (least-coupled endpoints, lowest-traffic routes)
3. **Migrate incrementally** — Move one seam at a time, verify, then move the next
4. **Route traffic** — Redirect completed seams through the facade to the new system
5. **Retire old code** — Delete migrated old-system code as each seam completes

### When It Breaks Down

- When old and new systems share state (session, cache) with incompatible formats
- When the old system is too coupled to extract individual seams
- When the team doesn't have bandwidth to maintain two systems simultaneously

### Example: Express → Fastify

```
Phase 1: Deploy Fastify alongside Express (different port/container)
Phase 2: Route /api/v2/* to Fastify, /api/v1/* stays on Express
Phase 3: Migrate endpoints one by one from v1 → v2
Phase 4: Update clients to use v2 endpoints
Phase 5: Retire Express when v1 traffic is zero
```

---

## Big Bang Rewrite

**Best for:** Small codebases (<50 files), prototypes, greenfield-with-data
**Risk:** High
**Duration:** Days to weeks

### When to Use

- The codebase is genuinely small and well-tested
- The current system has such deep technical debt that incremental migration is more expensive than rewrite
- A hard deadline requires a clean cutover (rare legitimate use case)
- You have comprehensive integration tests to validate the rewrite

### When NOT to Use

Almost always avoid big bang for production systems. The literature is clear: big bang rewrites fail more often than they succeed. They almost always:
- Take 3–5x longer than estimated
- Introduce regressions in edge cases the team forgot about
- Create a "second system effect" with over-engineered new features

### If You Must Do It

1. Write integration tests against the **old** system first — these become your acceptance suite
2. Freeze feature development on the old system during the rewrite window
3. Plan a hard rollback date — if the rewrite isn't ready by X, you revert and try an incremental approach
4. Run old and new in parallel for at least 2 weeks before cutover

---

## Parallel Run

**Best for:** Payment systems, data pipelines, financial calculations, high-stakes logic
**Risk:** Low to medium (for users), High (resource cost)
**Duration:** Weeks

### How It Works

Both old and new systems process every request. Outputs are compared. Discrepancies are logged and investigated before cutover.

```
Request
  │
  ├─→ Old System → Response (sent to user)
  └─→ New System → Response (compared, discarded)

Comparison Layer:
  Match: ✅ increment confidence counter
  Mismatch: ⚠️ log to investigation queue
```

Cutover happens when discrepancy rate drops to an acceptable threshold (typically <0.01%).

### Implementation

```python
async def handle_request(req):
    # Primary (old system) — controls user response
    old_response = await old_system.process(req)

    # Shadow (new system) — runs in background
    asyncio.create_task(shadow_compare(req, old_response))

    return old_response

async def shadow_compare(req, old_response):
    new_response = await new_system.process(req)
    if old_response != new_response:
        await log_discrepancy(req, old_response, new_response)
```

### Cutover Decision

Track discrepancy rate over time. Set explicit thresholds:
```
Week 1: 5% discrepancy rate — investigate high-volume paths
Week 2: 0.5% — investigate remaining edge cases
Week 3: 0.01% — acceptable for cutover
```

---

## Branch by Abstraction

**Best for:** Database migrations, auth system replacement, ORM swaps, cache layer changes
**Risk:** Low (if abstraction is well-designed)
**Duration:** Weeks

### How It Works

1. Introduce an abstraction (interface) over the component you want to replace
2. All code accesses the component through the abstraction
3. Build the new implementation behind the same interface
4. Swap implementations (old → new) with a feature flag
5. Remove the old implementation when stable

### Example: Sequelize → Prisma

```typescript
// Step 1: Define abstraction
interface UserRepository {
  findById(id: string): Promise<User>;
  create(data: CreateUserDto): Promise<User>;
  update(id: string, data: UpdateUserDto): Promise<User>;
  delete(id: string): Promise<void>;
}

// Step 2: Old implementation
class SequelizeUserRepository implements UserRepository {
  async findById(id: string) { /* Sequelize logic */ }
  // ...
}

// Step 3: New implementation
class PrismaUserRepository implements UserRepository {
  async findById(id: string) { /* Prisma logic */ }
  // ...
}

// Step 4: Swap with feature flag
const userRepo = featureFlags.prisma
  ? new PrismaUserRepository(prisma)
  : new SequelizeUserRepository(sequelize);
```

### When It Breaks Down

- When the existing code directly uses ORM-specific features (transactions across repositories, raw queries)
- When the abstraction layer itself becomes a performance bottleneck
- When the team resists the abstraction overhead for a small codebase

---

## Layer-by-Layer Migration

**Best for:** Full-stack transformations, language migrations, complete framework swaps
**Risk:** Medium (each layer introduces regression risk)
**Duration:** Months

### Layer Execution Order (safest first)

```
Layer 1: Infrastructure (Dockerfile, CI/CD, env vars)
          ↓ Validate: deploys correctly, env vars load
Layer 2: Database (schema, ORM, migrations)
          ↓ Validate: data integrity checks pass
Layer 3: Data Access (repositories, queries)
          ↓ Validate: all data ops return correct results
Layer 4: Business Logic (services, domain objects)
          ↓ Validate: unit tests pass on new layer
Layer 5: API Layer (routes, controllers, middleware)
          ↓ Validate: integration tests pass
Layer 6: Authentication (sessions, JWT, OAuth)
          ↓ Validate: auth flows work end-to-end
Layer 7: UI / Frontend (components, state, routing)
          ↓ Validate: E2E tests pass
Layer 8: Cleanup (remove old layer, update docs)
```

### Why This Order

Database-first because everything else depends on it. Auth late because it touches every layer and is high-risk. UI last because frontend can be migrated independently with less production risk.

---

## Database-First Migration

**Best for:** DB platform migrations (MySQL → PostgreSQL, MongoDB → PostgreSQL, Firebase → Supabase)
**Risk:** High (data integrity, no easy rollback once data mutates)
**Duration:** Weeks to months

### Critical Rule

**Never modify production data without a tested rollback.** Every schema change must have a corresponding down migration.

### Steps

1. **Audit schema** — Document every table, column, constraint, index, trigger, stored procedure
2. **Map types** — Create explicit source → target type mapping (watch out for: string lengths, decimal precision, datetime timezone handling, JSON support)
3. **ETL dry run** — Run migration against a production copy, measure time, check row counts
4. **Dual-write period** — Write to both databases simultaneously (old as primary)
5. **Read migration** — Gradually shift reads to new database (verify with parallel run)
6. **Write migration** — Flip writes to new database (old becomes replica)
7. **Cutover** — Retire old database

### Database-Specific Gotchas

**MySQL → PostgreSQL:**
- `AUTO_INCREMENT` → `SERIAL` or `GENERATED ALWAYS AS IDENTITY`
- Case-insensitive string comparison by default → case-sensitive in Postgres
- `TINYINT(1)` used as boolean → use `BOOLEAN` in Postgres
- Zero dates (`0000-00-00`) → invalid in Postgres

**MongoDB → PostgreSQL:**
- Embedded documents → normalised tables (or `JSONB` columns for semi-structured data)
- Arrays → junction tables or `ARRAY` columns
- No joins → requires application query changes
- ObjectId → `UUID` or `BIGINT`

**Firebase Firestore → Supabase:**
- Subcollections → related tables with foreign keys
- Security rules → Row Level Security (RLS) policies
- Realtime listeners → Supabase Realtime subscriptions
- Firebase Auth → Supabase Auth (user migration required)

---

## Traffic Splitting Strategy

**Best for:** API version migrations, framework migrations with stable APIs
**Risk:** Low

### Implementation Options

**nginx upstream split:**
```nginx
upstream backend {
    server old-system:3000 weight=80;
    server new-system:3000 weight=20;
}
```

**Feature flag service (LaunchDarkly, Unleash, etc.):**
```typescript
if (await flags.isEnabled('new-stack', user)) {
  return await newSystem.handle(req);
} else {
  return await oldSystem.handle(req);
}
```

**Header-based routing (for API clients you control):**
```
X-Stack-Version: new → route to new system
X-Stack-Version: old (or missing) → route to old system
```

### Traffic Split Schedule

```
Week 1:  5% → new system  (internal team only)
Week 2: 20% → new system  (beta users)
Week 3: 50% → new system  (50/50 split)
Week 4: 80% → new system  (validate at scale)
Week 5: 100% → new system (cutover)
Week 6: Retire old system
```

---

## Feature Flag Rollouts

For any migration that touches user-facing behaviour, use feature flags to control exposure independently of deployment.

### Flag Design for Migrations

```typescript
// Coarse flag: entire new stack
flags.newStack: boolean

// Fine-grained flags: per-subsystem
flags.newAuthSystem: boolean
flags.newPaymentFlow: boolean
flags.newSearchEngine: boolean
```

### Kill Switch Pattern

Every migration phase should have a kill switch — a flag that can be flipped to revert to old behaviour in under 60 seconds, without a deployment.

```typescript
if (!flags.killSwitch.newPaymentFlow) {
  return await newPaymentSystem.charge(req);
} else {
  return await oldPaymentSystem.charge(req);  // emergency fallback
}
```

---

## Anti-Patterns to Avoid

### ❌ The Boil-the-Ocean Rewrite

Trying to migrate everything at once. Classic failure mode. Always decompose into phases.

### ❌ Migrating Without Tests

If the codebase has <20% test coverage, write integration tests against the **old** system first. These become the acceptance suite for the new system.

### ❌ Schema Migration Before Application Migration

Changing the database schema before the application code is updated. This guarantees a broken state window. Always use expand-contract:
1. Expand schema (add new columns, keep old)
2. Deploy application that writes to both
3. Migrate data
4. Contract schema (drop old columns)
5. Deploy application that uses only new schema

### ❌ Migrating Auth Mid-Stream

Auth touches everything. Migrate it last, as a discrete phase with thorough testing. Mid-stream auth migration invalidates all sessions and breaks downstream integrations.

### ❌ Skipping the NO-GO Option

Engineers are optimists. Always do the honest assessment. Sometimes the right answer is "don't migrate" and it saves months of wasted effort.

### ❌ No Rollback Plan

Every phase needs an explicit rollback strategy defined before the phase starts. Writing rollback plans after something breaks is too late.

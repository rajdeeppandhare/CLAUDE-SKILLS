# Stack Mappings Reference

Detailed migration guidance for the most common source → target combinations.

---

## Table of Contents

1. [Language Migrations](#language-migrations)
   - JavaScript → TypeScript
   - Python → Go
   - Java → Kotlin
   - Python → Rust
2. [Framework Migrations](#framework-migrations)
   - React → Next.js
   - Flask/FastAPI → Go (Fiber/Chi/Gin)
   - Express → Fastify
   - Django → FastAPI
   - Angular → React
3. [Database Migrations](#database-migrations)
   - MongoDB → PostgreSQL
   - MySQL → PostgreSQL
   - Firebase → Supabase
   - SQLite → PostgreSQL
4. [Architecture Migrations](#architecture-migrations)
   - Monolith → Microservices
   - REST → GraphQL
   - Sync → Event-Driven
5. [Infrastructure Migrations](#infrastructure-migrations)
   - Firebase → Supabase (full platform)
   - AWS → GCP
   - Docker → Kubernetes

---

## Language Migrations

### JavaScript → TypeScript

**Recommended Pattern:** Layer-by-Layer (file by file)
**Estimated Effort:** 1–3 weeks for a 100-file project
**Breaking Risk:** Low (TypeScript is a superset)

**Core Tooling Setup:**
```bash
npm install -D typescript @types/node ts-node
npx tsc --init
```

**Recommended tsconfig.json (migration-safe):**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": false,        // start loose, tighten later
    "allowJs": true,        // keeps .js files working
    "checkJs": false,       // no errors on .js files yet
    "outDir": "./dist",
    "rootDir": "./src",
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

**Migration Order:**
```
1. tsconfig setup + allowJs (zero changes to code)
2. Rename files: .js → .ts (start with utils/helpers)
3. Add types to shared interfaces and DTOs
4. Type the API layer (request/response shapes)
5. Type service layer functions
6. Type React components (if applicable)
7. Add third-party @types/* packages
8. Enable strict: true (last step — fixes remaining errors)
```

**Common Problem Patterns:**

| JS Pattern | TS Equivalent |
|---|---|
| `const obj = {}` then adding props | Define interface first |
| `function foo(data)` | `function foo(data: FooInput): FooOutput` |
| `require('./thing')` | `import thing from './thing'` |
| Dynamic object keys | `Record<string, ValueType>` or index signature |
| `any` type abuse | Progressive: `unknown` → narrowed type |

**High-Risk Files to Flag:**
- Files using `eval()`
- Files with highly dynamic object construction
- Files using `arguments` object
- Files mixing CommonJS and ESM

---

### Python → Go

**Recommended Pattern:** Strangler Fig (endpoint by endpoint)
**Estimated Effort:** 4–12 weeks depending on complexity
**Breaking Risk:** Medium (language paradigm shift)

**Framework Mapping:**

| Python | Go Equivalent |
|---|---|
| FastAPI / Flask | Fiber, Chi, or Gin |
| Pydantic models | Go structs with validation tags |
| SQLAlchemy | GORM, sqlx, or pgx |
| Alembic | golang-migrate or goose |
| Celery | asynq, river, or machinery |
| Redis-py | go-redis |
| boto3 | aws-sdk-go-v2 |
| httpx / requests | net/http (stdlib) or resty |
| pytest | testing (stdlib) + testify |
| Poetry / pip | Go modules |

**Critical Paradigm Shifts:**
```
Python: Exceptions     → Go: Explicit error returns (if err != nil)
Python: None           → Go: Pointers (*T) or zero values
Python: Duck typing    → Go: Interfaces (explicit)
Python: Decorators     → Go: Middleware functions
Python: Generators     → Go: Channels or iterators
Python: Async/await    → Go: Goroutines + channels
Python: Dict           → Go: map[string]interface{} or struct
```

**Recommended Go Project Structure:**
```
cmd/
  api/main.go          ← entry point
internal/
  handler/             ← HTTP handlers (equiv: controllers/routes)
  service/             ← business logic (equiv: services)
  repository/          ← data access (equiv: models/db)
  domain/              ← entities and interfaces
  middleware/          ← auth, logging, rate limiting
pkg/
  config/              ← app configuration
  database/            ← DB connection pool
  errors/              ← custom error types
migrations/            ← SQL migration files
```

**Migration Phase Recommended:**
```
Phase 1: Set up Go project, Dockerfile, CI/CD
Phase 2: Database layer (GORM models mirror SQLAlchemy models)
Phase 3: Auth middleware (JWT decode/validate)
Phase 4: Migrate lowest-traffic endpoints first
Phase 5: Migrate core business logic endpoints
Phase 6: Migrate background jobs (Celery → asynq)
Phase 7: Traffic split (nginx weights)
Phase 8: Full cutover, retire Python
```

---

### Java → Kotlin

**Recommended Pattern:** File-by-file (Kotlin is JVM-compatible)
**Estimated Effort:** 1–4 weeks
**Breaking Risk:** Very Low (100% interop)

IntelliJ has an automated Java → Kotlin converter. Use it as a starting point, not as final output. The converter produces valid Kotlin but not idiomatic Kotlin.

**Key Improvements to Make Post-Conversion:**
```
Replace: if (x != null) { x.doThing() }
With:    x?.doThing()

Replace: Optional<T>
With:    T? (nullable type)

Replace: verbose getters/setters
With:    data class with properties

Replace: static utility classes
With:    top-level functions or extension functions

Replace: Streams API
With:    Kotlin collection operations (map, filter, fold)
```

---

### Python → Rust

**Recommended Pattern:** Big Bang (for performance-critical services only)
**Estimated Effort:** 3–6 months
**Breaking Risk:** High — consider this a full rewrite

**Honest Assessment:** This migration is only justified for:
- CPU-bound workloads with proven Python performance bottlenecks
- Systems where memory safety is a hard requirement
- Embedded or systems-level code

For most web services, Go is a better migration target than Rust.

**If Proceeding:**

| Python | Rust Equivalent |
|---|---|
| FastAPI | Axum or Actix-web |
| SQLAlchemy | SeaORM or Diesel |
| Pydantic | serde + validator |
| Celery | Tokio tasks |
| pytest | cargo test |
| Dict | HashMap or struct |
| None | Option<T> |
| Exception | Result<T, E> |

---

## Framework Migrations

### React → Next.js

**Recommended Pattern:** Layer-by-Layer
**Estimated Effort:** 1–3 weeks
**Breaking Risk:** Low–Medium

**Pre-Migration Assessment:**
```yaml
Check for:
  - React Router version (v5 vs v6 — different migration path)
  - Redux usage (can conflict with RSC)
  - Any SSR already implemented (react-helmet, etc.)
  - Code splitting approach (React.lazy vs Next.js automatic)
  - Image handling (migrate to next/image)
  - API calls in client (migrate to API routes or server components)
```

**Route Mapping:**

| React Router | Next.js App Router |
|---|---|
| `/` (index route) | `app/page.tsx` |
| `/about` | `app/about/page.tsx` |
| `/users/:id` | `app/users/[id]/page.tsx` |
| `/users/:id/posts/:postId` | `app/users/[id]/posts/[postId]/page.tsx` |
| `*` (404) | `app/not-found.tsx` |

**Component Classification (Server vs Client):**
```
Server Components (default in App Router):
  - Data fetching components
  - Static layout components
  - Components that don't use browser APIs

Client Components (add 'use client'):
  - Components using useState, useEffect
  - Components using browser APIs (window, document)
  - Components using event handlers
  - Components using React context
```

**Migration Steps:**
```
1. Install Next.js alongside React app (no code changes yet)
2. Move pages/ routes one at a time from React Router → App Router
3. Migrate data fetching: useEffect + fetch → async server components
4. Migrate image tags: <img> → <Image> from next/image
5. Migrate head management: react-helmet → next/head or metadata API
6. Migrate API calls to Next.js API routes or Route Handlers
7. Remove React Router, serve via Next.js routing only
8. Enable Vercel/Next.js deployment
```

---

### Express → Fastify

**Recommended Pattern:** Strangler Fig
**Estimated Effort:** 1–2 weeks for a 30-endpoint API
**Breaking Risk:** Low

**API Mapping:**

| Express | Fastify |
|---|---|
| `app.get('/path', handler)` | `fastify.get('/path', handler)` |
| `req.params.id` | `request.params.id` |
| `req.body` | `request.body` |
| `res.json(data)` | `reply.send(data)` |
| `res.status(400).json(err)` | `reply.status(400).send(err)` |
| `app.use(middleware)` | `fastify.addHook('onRequest', hook)` |
| `express.json()` | Built-in (JSON default) |
| `cors` package | `@fastify/cors` |
| `helmet` package | `@fastify/helmet` |
| `passport.js` | `@fastify/passport` |

**Key Advantage to Leverage:**
Fastify's schema validation with JSON Schema eliminates runtime type errors and provides automatic serialization performance gains.

```typescript
// Express (no schema)
app.post('/users', (req, res) => {
  const { name, email } = req.body;  // unvalidated
});

// Fastify (schema-first)
fastify.post('/users', {
  schema: {
    body: {
      type: 'object',
      required: ['name', 'email'],
      properties: {
        name: { type: 'string' },
        email: { type: 'string', format: 'email' }
      }
    }
  }
}, async (request, reply) => {
  const { name, email } = request.body;  // validated + typed
});
```

---

### Flask/FastAPI → Go (Fiber)

**Recommended Pattern:** Strangler Fig
**Estimated Effort:** 4–8 weeks
**Breaking Risk:** Medium

See Python → Go section above for full mapping table.

**FastAPI-Specific Mapping:**

| FastAPI | Go Fiber |
|---|---|
| `@app.get("/path")` | `app.Get("/path", handler)` |
| Pydantic `BaseModel` | Go struct with binding tags |
| `Depends()` | Middleware or service injection |
| `BackgroundTasks` | Goroutines |
| `HTTPException` | `c.Status(400).JSON(err)` |
| OpenAPI auto-docs | `github.com/gofiber/swagger` |
| Async handlers | All handlers in Fiber are sync (goroutines implicit) |

---

### Angular → React

**Recommended Pattern:** Big Bang or Strangler Fig (by feature module)
**Estimated Effort:** 6–16 weeks for large apps
**Breaking Risk:** High (different component model, different state management)

**Concept Mapping:**

| Angular | React |
|---|---|
| Component | Function Component |
| Template | JSX |
| NgModule | (no direct equivalent — use file organisation) |
| Service (injectable) | Context + hook, or Zustand/Redux store |
| `@Input()` | props |
| `@Output()` / EventEmitter | Callback props |
| Two-way binding `[(ngModel)]` | Controlled inputs: `value` + `onChange` |
| Router (RouterModule) | React Router or TanStack Router |
| HttpClient | fetch, axios, or TanStack Query |
| Guards (CanActivate) | Protected Route wrapper component |
| Pipes | Utility functions or custom hooks |
| Directive | Custom hook or wrapper component |
| Lifecycle `ngOnInit` | `useEffect(fn, [])` |
| Lifecycle `ngOnDestroy` | `useEffect` cleanup return |
| RxJS Observables | Promises, or `useQuery` from TanStack Query |

**State Management Recommendation:**
```
Simple local state:     useState
Shared UI state:        Zustand (simplest) or Jotai
Complex global state:   Redux Toolkit (closest to NgRx)
Server state:           TanStack Query (replaces most RxJS HTTP usage)
```

---

## Database Migrations

### MongoDB → PostgreSQL

**Recommended Pattern:** Branch by Abstraction → Dual Write → Cutover
**Estimated Effort:** 3–8 weeks
**Breaking Risk:** High (data model paradigm shift)

**Schema Design Questions to Answer First:**
1. Which embedded documents should become separate tables?
2. Which arrays should become junction tables?
3. Which fields are genuinely dynamic (→ JSONB column) vs structured (→ proper columns)?

**Type Mapping:**

| MongoDB | PostgreSQL |
|---|---|
| ObjectId | UUID (gen_random_uuid()) |
| String | TEXT or VARCHAR(n) |
| Number | INTEGER, BIGINT, NUMERIC, FLOAT8 |
| Boolean | BOOLEAN |
| Date | TIMESTAMPTZ |
| Array (of scalars) | ARRAY type or junction table |
| Array (of objects) | Junction table |
| Embedded document | Separate table (FK) or JSONB |
| Mixed/any schema | JSONB |

**Migration Tool: Recommended ETL Script Pattern:**
```python
import pymongo
import psycopg2

mongo = pymongo.MongoClient("mongodb://...")
pg = psycopg2.connect("postgresql://...")

for doc in mongo.db.users.find():
    pg.cursor().execute("""
        INSERT INTO users (id, email, name, created_at)
        VALUES (%s, %s, %s, %s)
        ON CONFLICT (id) DO NOTHING
    """, (
        str(doc['_id']),
        doc['email'],
        doc.get('name'),
        doc.get('createdAt')
    ))
```

---

### Firebase → Supabase

**Recommended Pattern:** Feature-by-feature migration
**Estimated Effort:** 2–6 weeks
**Breaking Risk:** Medium

**Service Mapping:**

| Firebase | Supabase |
|---|---|
| Firestore | PostgreSQL (via Supabase client) |
| Firebase Auth | Supabase Auth |
| Firebase Storage | Supabase Storage |
| Firebase Functions | Edge Functions (Deno) or PostgreSQL Functions |
| Security Rules | Row Level Security (RLS) |
| Firebase Realtime DB | Supabase Realtime |
| Firebase Analytics | (Use PostHog or Plausible) |
| Firebase Hosting | (Use Vercel/Netlify — Supabase doesn't host frontends) |

**Auth User Migration:**
```typescript
// Export Firebase users (Admin SDK)
const listAllUsers = async () => {
  const users = [];
  let pageToken;
  do {
    const result = await admin.auth().listUsers(1000, pageToken);
    users.push(...result.users);
    pageToken = result.pageToken;
  } while (pageToken);
  return users;
};

// Import to Supabase (via admin API)
// Note: passwords cannot be migrated — users must reset
```

**Security Rules → RLS Translation:**

```javascript
// Firebase Security Rule
match /users/{userId} {
  allow read, write: if request.auth.uid == userId;
}

// Supabase RLS equivalent
CREATE POLICY "users_own_data" ON users
  FOR ALL USING (auth.uid() = id);
```

---

## Architecture Migrations

### Monolith → Microservices

**Recommended Pattern:** Strangler Fig (always)
**Estimated Effort:** 6–18 months
**Breaking Risk:** Very High — most teams underestimate this

**STOP. Read this first.**

The most common engineering mistake is migrating to microservices too early. Microservices are a solution to organisational scale, not a technical improvement. Before proceeding, confirm:

- [ ] You have >50 engineers on this codebase
- [ ] Teams are blocked on each other for deployments
- [ ] You have proven that a monolith with good module boundaries can't solve the problem
- [ ] You have a platform/DevOps team to own the infrastructure overhead

If those aren't true, consider **modular monolith** instead.

**Service Boundary Identification:**

Use these signals to find natural service boundaries:
```
1. Data ownership — what tables does each domain own exclusively?
2. Team ownership — what does each team work on?
3. Change frequency — which modules change together?
4. Scale requirements — which parts need to scale independently?
5. Failure isolation — which parts should fail independently?
```

**Service Extraction Order:**
```
Extract first (lowest risk):
  - Read-only services (reporting, analytics)
  - Services with no dependencies on other domains
  - Services with clear, stable APIs

Extract last (highest risk):
  - Auth service (touches everything)
  - Payment service (financial risk)
  - Core domain service (everything depends on it)
```

---

### REST → GraphQL

**Recommended Pattern:** Parallel Run (keep REST, add GraphQL layer)
**Estimated Effort:** 2–6 weeks for schema design + 4–12 weeks for resolver implementation
**Breaking Risk:** Low (additive — existing REST clients unaffected)

**When REST is better than GraphQL:**
- Simple CRUD with predictable client needs
- Public API with external consumers
- High-performance caching requirements
- Team unfamiliar with GraphQL

**When GraphQL is better:**
- Multiple clients with different data needs (mobile vs web)
- Frequent under-fetching or over-fetching
- Rapid product iteration requiring flexible data access
- Complex, nested relational data

**Migration Strategy:**
```
1. Define GraphQL schema (types, queries, mutations)
2. Set up GraphQL server alongside REST (Apollo, Pothos, etc.)
3. Implement resolvers that call existing service layer (not REST endpoints)
4. Build new client features against GraphQL
5. Migrate existing REST-consuming features one at a time
6. Retire REST endpoints as usage drops to zero
```

---

## Infrastructure Migrations

### Docker → Kubernetes

**Recommended Pattern:** Parallel Run (shadow cluster)
**Estimated Effort:** 4–12 weeks
**Breaking Risk:** Medium

**Pre-Migration Checklist:**
- [ ] All services are containerised (required for K8s)
- [ ] Services are stateless (or state is externalised to DB/Redis)
- [ ] Health check endpoints exist (/health, /ready)
- [ ] Configuration is environment-variable based (12-factor)
- [ ] Logging goes to stdout/stderr (not files)
- [ ] You have a team member with K8s operational experience

**Component Mapping:**

| Docker Compose | Kubernetes |
|---|---|
| `service:` | Deployment + Service |
| `ports:` | Service (LoadBalancer or NodePort) |
| `volumes:` | PersistentVolume + PersistentVolumeClaim |
| `environment:` | ConfigMap + Secret |
| `healthcheck:` | livenessProbe + readinessProbe |
| `depends_on:` | Init containers or readiness gates |
| `networks:` | Namespace isolation |
| `replicas:` | Deployment `replicas:` |

**Migration Path:**
```
Phase 1: Write Kubernetes manifests (Deployment, Service, Ingress)
Phase 2: Test in local cluster (kind or minikube)
Phase 3: Deploy to staging K8s cluster
Phase 4: Run production load in shadow mode
Phase 5: Gradual traffic shift (10% → 50% → 100%)
Phase 6: Decommission Docker Compose production setup
```

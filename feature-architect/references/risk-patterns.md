# Risk Pattern Library

> Load this file during Phase 4 (Risk Detection) for complex features. Contains 80+ battle-tested risk patterns organised by category.

---

## Table of Contents

1. [Authentication & Authorisation Risks](#1-authentication--authorisation-risks)
2. [Database & Data Integrity Risks](#2-database--data-integrity-risks)
3. [API & Integration Risks](#3-api--integration-risks)
4. [Concurrency & Race Conditions](#4-concurrency--race-conditions)
5. [Security Risks](#5-security-risks)
6. [Payment & Billing Risks](#6-payment--billing-risks)
7. [Real-time & Event System Risks](#7-real-time--event-system-risks)
8. [File & Storage Risks](#8-file--storage-risks)
9. [Performance & Scaling Risks](#9-performance--scaling-risks)
10. [Deployment & Migration Risks](#10-deployment--migration-risks)
11. [Third-Party Service Risks](#11-third-party-service-risks)
12. [Testing & Observability Risks](#12-testing--observability-risks)

---

## 1. Authentication & Authorisation Risks

### R-AUTH-01: Insufficient Role Model
- **Trigger**: Feature requires permission checks the existing auth system doesn't support
- **Example**: Adding team collaboration when auth only has `isAdmin: boolean`
- **Impact**: HIGH — either over-permissive (security hole) or under-permissive (blocks legitimate users)
- **Mitigation**: Extend role model first; RBAC (role-based access control) is almost always the right pattern; never use boolean flags for more than 2 states

### R-AUTH-02: Session Scope Mismatch
- **Trigger**: Feature adds multi-tenancy (orgs, teams, workspaces) but session stores single user context
- **Example**: User is member of 3 orgs; session doesn't know which org is "active"
- **Impact**: HIGH — data leakage across tenants or broken feature behaviour
- **Mitigation**: Add `active_org_id` or similar to session; include org context in JWT claims or server session

### R-AUTH-03: Permission Check Bypass via API
- **Trigger**: New API endpoint doesn't have same auth middleware applied
- **Example**: `/api/admin/users` added without `requireAdmin` middleware
- **Impact**: CRITICAL — unauthenticated or unauthorised access
- **Mitigation**: Apply middleware at router level, not per-handler; audit every new endpoint added

### R-AUTH-04: Horizontal Privilege Escalation
- **Trigger**: Feature allows users to reference resources by ID (e.g., GET /api/documents/:id)
- **Example**: User A can access User B's document by guessing the ID
- **Impact**: HIGH — data exposure
- **Mitigation**: Always scope queries with `WHERE owner_id = current_user_id`; never trust client-supplied IDs without ownership check

### R-AUTH-05: Token Scope Creep
- **Trigger**: Adding OAuth third-party integration requests too many scopes
- **Example**: Requesting `write:calendar` when only `read:calendar` is needed
- **Impact**: MEDIUM — users decline permissions; unnecessary data access
- **Mitigation**: Request minimum required scopes; progressive permission requests (ask when needed)

### R-AUTH-06: Broken Object Level Authorisation (BOLA)
- **Trigger**: API exposes internal IDs in URLs and doesn't verify ownership
- **Example**: `/api/invoices/inv_123` returns invoice even if it belongs to another customer
- **Impact**: CRITICAL — OWASP API Security Top 1
- **Mitigation**: Every resource access must verify: `resource.owner_id === request.user.id`

### R-AUTH-07: Invitation Token Security
- **Trigger**: Team/org invite system added with email-based tokens
- **Example**: Invitation tokens don't expire; anyone with old email can join
- **Impact**: HIGH — stale invitations become security vulnerabilities
- **Mitigation**: Tokens expire (24–48h); single-use; tied to invited email address; revocable

---

## 2. Database & Data Integrity Risks

### R-DB-01: NOT NULL Column on Live Table
- **Trigger**: Adding a required column to an existing table with data
- **Example**: `ALTER TABLE users ADD COLUMN subscription_tier VARCHAR NOT NULL`
- **Impact**: HIGH — migration will fail or lock table in production
- **Mitigation**: Add as nullable first, backfill, then add NOT NULL constraint; or add with DEFAULT value

### R-DB-02: Missing Foreign Key Constraints
- **Trigger**: New table references existing table but relationship isn't enforced
- **Example**: `notifications.user_id` has no FK to `users.id`
- **Impact**: MEDIUM — orphaned records; broken joins; data integrity degradation over time
- **Mitigation**: Always add FK constraints; define ON DELETE behaviour (CASCADE vs RESTRICT vs SET NULL)

### R-DB-03: Orphaned Records on Delete
- **Trigger**: Feature adds child records but parent deletion doesn't clean them up
- **Example**: Deleting a workspace leaves orphaned projects, tasks, members
- **Impact**: MEDIUM — DB bloat; broken queries; privacy violations (deleted user data persists)
- **Mitigation**: Define cascade behaviour explicitly; consider soft deletes; test deletion paths

### R-DB-04: Missing Transaction Boundaries
- **Trigger**: Feature performs multiple related writes (create + update + insert)
- **Example**: Create subscription + update user + create invoice — three separate DB calls
- **Impact**: HIGH — partial failures leave data in inconsistent state
- **Mitigation**: Wrap multi-write operations in a transaction; use database-level atomicity guarantees

### R-DB-05: Index Gap
- **Trigger**: New query filters or sorts on unindexed column
- **Example**: `SELECT * FROM notifications WHERE user_id = ? ORDER BY created_at DESC` with no index
- **Impact**: MEDIUM → HIGH at scale — full table scans; slow queries; potential DB overload
- **Mitigation**: Add index for every column used in WHERE, JOIN, or ORDER BY on large tables

### R-DB-06: Schema Enum Evolution
- **Trigger**: Adding values to an existing database enum type
- **Example**: Adding 'enterprise' to `subscription_tier` enum in PostgreSQL
- **Impact**: MEDIUM — in PostgreSQL, ALTER TYPE requires exclusive lock; can't roll back enum values
- **Mitigation**: Use VARCHAR with CHECK constraint instead of native enums for extensible types; or use lookup tables

### R-DB-07: N+1 Query in New Feature
- **Trigger**: New UI shows a list where each item needs a separate DB lookup
- **Example**: Team member list shows "last active time" — queried per-member in a loop
- **Impact**: MEDIUM → HIGH — 1 page load = N+1 queries; catastrophic at scale
- **Mitigation**: Use eager loading / JOIN; batch queries; DataLoader pattern for GraphQL

### R-DB-08: Unbounded Result Set
- **Trigger**: New API endpoint or query doesn't paginate
- **Example**: GET /api/notifications returns all notifications ever for a user
- **Impact**: HIGH — memory overflow; timeout; DoS vector
- **Mitigation**: Always paginate; add LIMIT to every query; default page size with max cap

---

## 3. API & Integration Risks

### R-API-01: Breaking Change to Existing Endpoint
- **Trigger**: Modifying response shape of an endpoint that has existing consumers
- **Example**: Renaming `user.name` → `user.display_name` in `/api/me` response
- **Impact**: HIGH — breaks mobile apps, third-party integrations, existing frontend
- **Mitigation**: Version the API (v2); add fields without removing old ones; deprecation period

### R-API-02: Missing Input Validation
- **Trigger**: New endpoint accepts user-submitted data without validation
- **Example**: POST /api/teams accepts `name` with no length limit or sanitisation
- **Impact**: HIGH — XSS, injection, DB errors from oversized fields
- **Mitigation**: Validate all inputs (Zod, Joi, class-validator); enforce max lengths; sanitise strings

### R-API-03: Rate Limiting Gap
- **Trigger**: New endpoint added without rate limiting
- **Example**: POST /api/invite-user sends email — no rate limit means spam vector
- **Impact**: MEDIUM → HIGH — abuse, cost overruns (email API cost), DoS
- **Mitigation**: Apply rate limiting at middleware level for all mutation endpoints; tighter limits for email/SMS

### R-API-04: Webhook Endpoint Not Verified
- **Trigger**: Adding webhook receiver (Stripe, GitHub, Twilio, etc.) without signature verification
- **Example**: POST /api/webhooks/stripe processes any request without verifying Stripe signature
- **Impact**: CRITICAL — attacker can send fake payment events; trigger false subscription upgrades
- **Mitigation**: Always verify webhook signatures using the provider SDK; reject unverified requests

### R-API-05: CORS Misconfiguration
- **Trigger**: New API endpoints added without reviewing CORS policy
- **Example**: Wildcard CORS (`*`) on endpoints that return user data
- **Impact**: HIGH — cross-origin data exfiltration
- **Mitigation**: Restrict CORS to known origins; never use wildcard for authenticated endpoints

---

## 4. Concurrency & Race Conditions

### R-RACE-01: Double-Submit Pattern
- **Trigger**: UI action that triggers a payment, order, or account change has no debounce/disable
- **Example**: User clicks "Upgrade" twice quickly; two subscriptions created
- **Impact**: HIGH — duplicate charges; data integrity issues
- **Mitigation**: Disable button on first click; idempotency keys on payment API calls; DB unique constraints

### R-RACE-02: Check-Then-Act Race
- **Trigger**: Feature checks a condition then acts on it in two separate operations
- **Example**: Check if username is available, then insert user — another request can grab it between the two
- **Impact**: MEDIUM → HIGH — duplicate data, constraint violations, data corruption
- **Mitigation**: Use DB-level constraints (UNIQUE) as the true guard; use INSERT ... ON CONFLICT; use SELECT FOR UPDATE

### R-RACE-03: Webhook Replay
- **Trigger**: Webhook handler processes events without idempotency check
- **Example**: Stripe retries a payment webhook 3 times; subscription upgraded 3 times
- **Impact**: HIGH — duplicate processing; incorrect billing; data corruption
- **Mitigation**: Store processed event IDs; check before processing; use UPSERT not INSERT for event-driven state changes

### R-RACE-04: Subscription Upgrade Mid-Cycle
- **Trigger**: User upgrades while an existing billing cycle operation is in progress
- **Example**: Proration calculated at T=0, webhook arrives at T=0.5 with new plan, amounts mismatch
- **Impact**: MEDIUM — incorrect billing amounts; subscription state desync
- **Mitigation**: Use Stripe's built-in proration; lock subscription record during updates; reconcile via webhook events

### R-RACE-05: Concurrent File Upload
- **Trigger**: Multiple users upload files with the same generated filename
- **Example**: User avatar stored at `uploads/{user_id}.jpg` — concurrent uploads overwrite each other
- **Impact**: MEDIUM — data loss; wrong avatar displayed
- **Mitigation**: Use UUID or timestamp in filenames; never use predictable paths

---

## 5. Security Risks

### R-SEC-01: Server-Side Request Forgery (SSRF)
- **Trigger**: Feature accepts a URL from user and fetches it server-side
- **Example**: "Import from URL" feature; user submits `http://169.254.169.254/latest/meta-data/`
- **Impact**: CRITICAL — attacker can read cloud metadata, internal services, credentials
- **Mitigation**: Allowlist valid domains; block private IP ranges; never follow redirects to private addresses

### R-SEC-02: Mass Assignment
- **Trigger**: Object created/updated directly from request body without field filtering
- **Example**: PATCH /api/users spreads `req.body` directly onto user object — attacker sets `isAdmin: true`
- **Impact**: CRITICAL — privilege escalation
- **Mitigation**: Explicitly pick allowed fields from request body; never spread request body onto DB models

### R-SEC-03: Sensitive Data in Logs
- **Trigger**: Logging request/response bodies that may contain passwords, tokens, or PII
- **Example**: `console.log('User created:', user)` where user object contains password hash
- **Impact**: MEDIUM → HIGH — credential/PII leakage in log management systems
- **Mitigation**: Redact sensitive fields before logging; use structured logging with explicit field selection

### R-SEC-04: Environment Variable Exposure
- **Trigger**: New secrets added to project but not documented; accidental client-side exposure
- **Example**: Next.js — any env var without `NEXT_PUBLIC_` prefix stays server-side, but developers sometimes add API keys with the prefix accidentally
- **Impact**: CRITICAL — API key/secret exposure
- **Mitigation**: Audit `.env.example`; never prefix secrets with `NEXT_PUBLIC_`; rotate any accidentally exposed keys

### R-SEC-05: SQL Injection via ORM Bypass
- **Trigger**: Raw SQL queries used alongside ORM, with user input interpolated
- **Example**: `db.query('SELECT * FROM users WHERE name = ' + req.body.name)`
- **Impact**: CRITICAL
- **Mitigation**: Never interpolate user input into SQL; use parameterised queries or ORM methods exclusively

### R-SEC-06: Insecure Direct Object Reference via File Path
- **Trigger**: Feature serves files from disk using user-supplied path
- **Example**: GET /api/files?path=../../../etc/passwd
- **Impact**: CRITICAL — path traversal, file system exposure
- **Mitigation**: Never use user input in file paths; map to UUIDs; validate against allowlist

---

## 6. Payment & Billing Risks

### R-PAY-01: Trial Abuse
- **Trigger**: Free trial with no credit card requirement
- **Example**: Users create new accounts repeatedly to keep getting trials
- **Impact**: MEDIUM — revenue loss; abuse
- **Mitigation**: Require payment method upfront (charge at end); device fingerprinting; email domain deduplication

### R-PAY-02: Subscription State Desync
- **Trigger**: Subscription status stored in your DB but source of truth is Stripe
- **Example**: Stripe cancels subscription due to failed payment; your DB still shows 'active'
- **Impact**: HIGH — users retain access they shouldn't; revenue loss
- **Mitigation**: Webhook handlers are the only thing that updates subscription status; never compute it client-side

### R-PAY-03: Failed Payment Handling
- **Trigger**: No retry logic or dunning flow for failed payments
- **Example**: Card declines; subscription silently becomes inactive with no notification
- **Impact**: HIGH — involuntary churn
- **Mitigation**: Handle `invoice.payment_failed` webhook; implement dunning emails; grace period before downgrade

### R-PAY-04: Currency / Tax Compliance
- **Trigger**: Expanding to international customers without tax handling
- **Example**: UK customers not charged VAT; EU customers not charged applicable tax
- **Impact**: HIGH — legal/compliance liability
- **Mitigation**: Use Stripe Tax or equivalent; store customer country; handle tax-exempt status

### R-PAY-05: Refund Race Condition
- **Trigger**: Refund processed while subscription is being renewed
- **Example**: Refund for current period issued same day as renewal — both succeed, customer gets double credit
- **Impact**: MEDIUM — revenue loss
- **Mitigation**: Check subscription state before issuing refund; process refunds only on finalised invoices

---

## 7. Real-time & Event System Risks

### R-RT-01: WebSocket Authentication
- **Trigger**: WebSocket connection not authenticated
- **Example**: Any client can connect to `ws://app.com/notifications` and receive all events
- **Impact**: CRITICAL — data exposure
- **Mitigation**: Authenticate WebSocket handshake; validate token on connection; associate connection to user

### R-RT-02: Fan-out at Scale
- **Trigger**: Event broadcasted to all connected users
- **Example**: Team notification sent to all 10,000 team members via individual WebSocket writes
- **Impact**: HIGH — server memory exhaustion; event delivery delays
- **Mitigation**: Use pub/sub (Redis, Ably, Pusher); never loop over connections in application code

### R-RT-03: Message Order Guarantee
- **Trigger**: Feature depends on events being processed in order
- **Example**: Document edit events must be applied in sequence; if two arrive out of order, document corrupts
- **Impact**: HIGH — data corruption
- **Mitigation**: Include sequence numbers; use ordered queues (SQS FIFO, Kafka partitions); implement optimistic concurrency control

### R-RT-04: Connection Leak
- **Trigger**: WebSocket connections not cleaned up on disconnect or error
- **Example**: Server accumulates dead WebSocket connections; memory grows unboundedly
- **Impact**: HIGH — memory leak; eventual OOM crash
- **Mitigation**: Handle `close` and `error` events; remove connection from tracking set; implement heartbeat/ping-pong

---

## 8. File & Storage Risks

### R-FILE-01: Missing File Size Limit
- **Trigger**: File upload endpoint with no size restriction
- **Example**: User uploads 10GB file; server runs out of disk/memory; DoS
- **Impact**: HIGH
- **Mitigation**: Enforce size limits at the CDN/load balancer level AND in application code; use pre-signed S3 URLs with Content-Length restriction

### R-FILE-02: File Type Bypass
- **Trigger**: File type validated by extension only, not MIME type or magic bytes
- **Example**: Attacker renames `malware.exe` to `photo.jpg`; server accepts it
- **Impact**: HIGH — stored malware; XSS via SVG upload
- **Mitigation**: Validate MIME type; inspect magic bytes for images; never execute uploaded files; serve from separate origin

### R-FILE-03: Public Bucket Misconfiguration
- **Trigger**: S3 bucket or cloud storage set to public for uploaded user files
- **Example**: User uploads sensitive documents; accessible at predictable URL to anyone
- **Impact**: CRITICAL — data exposure
- **Mitigation**: Default to private storage; use signed URLs with expiry for access; never make user-upload buckets public

### R-FILE-04: Filename Collision
- **Trigger**: Files stored using original filename rather than generated ID
- **Example**: Two users upload `avatar.jpg`; second overwrites first
- **Impact**: MEDIUM — data loss; wrong content served
- **Mitigation**: Always generate a UUID or content-hash-based filename; keep original name as metadata only

---

## 9. Performance & Scaling Risks

### R-PERF-01: Synchronous External API Call in Request Path
- **Trigger**: External API call (Stripe, SendGrid, Slack) made inline during request handling
- **Example**: POST /api/signup calls SendGrid synchronously; if SendGrid is slow, signup is slow
- **Impact**: MEDIUM — high latency; timeout risk; poor UX
- **Mitigation**: Queue non-critical external calls (welcome email, Slack notification); only block on critical ones (payment)

### R-PERF-02: Missing Cache Layer
- **Trigger**: Expensive computation or query repeated on every request
- **Example**: Permission check queries role table on every API request
- **Impact**: MEDIUM — DB overload; high latency at scale
- **Mitigation**: Cache permission/role lookups in Redis or in-memory with short TTL; cache expensive aggregations

### R-PERF-03: Large Payload Response
- **Trigger**: API returns entire nested object when only a subset is needed
- **Example**: GET /api/dashboard returns full user object, all projects, all team members, all recent activity — on every load
- **Impact**: MEDIUM — slow page loads; mobile data consumption
- **Mitigation**: Return only required fields; use projection in DB queries; separate heavy analytics endpoints

### R-PERF-04: Job Queue Not Implemented
- **Trigger**: Feature requires async processing (sending emails, processing images, generating reports) but runs synchronously
- **Example**: PDF report generation runs inline; request times out after 30s
- **Impact**: HIGH — timeouts; poor UX; retry storms
- **Mitigation**: Use job queue (Bull, Sidekiq, Celery, AWS SQS); return 202 Accepted + job ID; poll or webhook for completion

---

## 10. Deployment & Migration Risks

### R-DEPLOY-01: Zero-Downtime Migration Not Possible
- **Trigger**: DB migration requires locking the table that handles high traffic
- **Example**: Adding an index to `orders` table (10M rows) with `CREATE INDEX` — full table lock
- **Impact**: HIGH — downtime during deploy
- **Mitigation**: Use `CREATE INDEX CONCURRENTLY` in PostgreSQL; schedule migrations during low traffic; use expand-contract pattern

### R-DEPLOY-02: Backward-Incompatible Migration
- **Trigger**: Migration removes or renames a column that running application code still reads
- **Example**: Deploy renames `user.name` → `user.display_name` but old pod still reads `.name`
- **Impact**: HIGH — runtime errors during rolling deploy
- **Mitigation**: Expand-contract: add new column → deploy code that writes both → migrate data → deploy code that reads new only → drop old column

### R-DEPLOY-03: Missing Feature Flag
- **Trigger**: Major feature shipped to 100% of users simultaneously
- **Example**: New billing UI deployed; bug in Stripe integration affects all users immediately
- **Impact**: HIGH — no rollback mechanism
- **Mitigation**: Use feature flags (LaunchDarkly, Unleash, simple DB flag); roll out to 1% → 10% → 100%

### R-DEPLOY-04: Secrets Not Provisioned
- **Trigger**: New feature requires new environment variables not yet added to production
- **Example**: Feature works locally (has `.env`); crashes in production (no env var set)
- **Impact**: HIGH — production outage on deploy
- **Mitigation**: Update `.env.example` as part of the feature PR; document required secrets in DEPLOYMENT.md; check env vars on startup

---

## 11. Third-Party Service Risks

### R-3P-01: No Fallback on Service Outage
- **Trigger**: Critical path depends on third-party service with no fallback
- **Example**: Auth depends entirely on Auth0; if Auth0 is down, nobody can log in
- **Impact**: HIGH — total service unavailability
- **Mitigation**: Implement circuit breakers; graceful degradation; fallback logic; alert on third-party failures

### R-3P-02: API Rate Limit Breach
- **Trigger**: Feature calls third-party API once per user action, at scale this exceeds limits
- **Example**: Sending a Slack notification on every user activity event; 1000 events/min exceeds Slack rate limit
- **Impact**: MEDIUM — feature silently stops working; hard to debug
- **Mitigation**: Batch API calls; respect `Retry-After` headers; implement exponential backoff; cache responses

### R-3P-03: Vendor Lock-in Without Abstraction
- **Trigger**: Third-party SDK used directly throughout codebase with no abstraction layer
- **Example**: Stripe SDK calls scattered across 15 files
- **Impact**: LOW → MEDIUM — difficult to switch providers; testing requires mocking SDK
- **Mitigation**: Wrap third-party calls in a service layer; define interface contract; inject dependency

### R-3P-04: Webhook IP Not Allowlisted
- **Trigger**: Webhook endpoint doesn't verify source IP against provider's published range
- **Example**: Stripe publishes their webhook source IPs; not checking allows spoofed webhooks
- **Impact**: MEDIUM — additional attack surface (signature verification is the primary defence)
- **Mitigation**: Signature verification is sufficient as primary; IP allowlisting is defence-in-depth

---

## 12. Testing & Observability Risks

### R-OBS-01: No Error Tracking
- **Trigger**: Feature added without error monitoring integration
- **Example**: Payment webhook fails silently; nobody knows for 3 days
- **Impact**: HIGH — silent failures; difficult incident response
- **Mitigation**: Instrument with Sentry/Bugsnag/Datadog; capture errors in background jobs especially; set up alerts

### R-OBS-02: No Audit Log
- **Trigger**: Sensitive actions (payment, permission change, data deletion) have no audit trail
- **Example**: Admin deletes user; no record of who did it or when
- **Impact**: MEDIUM → HIGH — compliance risk; difficult debugging; no forensics on incidents
- **Mitigation**: Log sensitive actions to an audit table: `(actor, action, target, timestamp, metadata)`

### R-OBS-03: Test Environment Parity
- **Trigger**: Feature tested in dev but has different behaviour in production
- **Example**: Redis queue works in dev (single instance); in production (cluster mode) routing is different
- **Impact**: MEDIUM — works locally, breaks in production
- **Mitigation**: Use Docker Compose to match production services in dev; test against staging before prod

### R-OBS-04: Missing Integration Tests
- **Trigger**: Feature involves multiple services but only unit tests written
- **Example**: Webhook → billing service → subscription update chain tested only unit-by-unit; integration never tested
- **Impact**: MEDIUM — integration bugs caught only in production
- **Mitigation**: Write at least one end-to-end test for each critical user journey; use Stripe test mode, etc.

---

*Reference: Load this file in Phase 4 when features are Medium or higher complexity. For Low complexity features, apply the most relevant sections inline.*

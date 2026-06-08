# CAG — Query Playbook

Reference file for advanced query modes: code review, security audit, refactor planning,
feature planning, and performance analysis. Load when user asks for any of these.

---

## Playbook 1 — Security Audit

Full security sweep of the codebase using the CKM. Perform this systematically.

### Audit Checklist by Category

**Authentication & Session Management**
- [ ] Are passwords hashed with bcrypt/argon2/scrypt? (never MD5/SHA1)
- [ ] JWT secrets stored in env vars, not hardcoded?
- [ ] Token expiry set? (access + refresh token lifetimes)
- [ ] Refresh token rotation implemented?
- [ ] Session invalidation on logout?
- [ ] Account lockout on failed login attempts?

**Authorisation**
- [ ] Are all endpoints protected that should be?
- [ ] Is auth middleware applied globally or per-route? (per-route = risk of gaps)
- [ ] RBAC / permissions — are they enforced at service layer, not just route layer?
- [ ] Horizontal privilege escalation possible? (user accessing other user's data via ID manipulation)

**Input Validation & Injection**
- [ ] SQL injection — raw query strings with user input?
- [ ] Command injection — child_process / exec / shell calls with user input?
- [ ] Path traversal — file operations with user-supplied paths?
- [ ] XSS — is user content rendered unescaped?
- [ ] NoSQL injection — MongoDB queries built from user input without sanitisation?
- [ ] Schema validation on all inputs? (Zod / Joi / Pydantic / class-validator)

**File Handling**
- [ ] File type validation (MIME + extension, not just extension)?
- [ ] File size limits enforced?
- [ ] Files stored outside web root or in object storage (not served directly)?
- [ ] Filename sanitisation before storage?

**API Security**
- [ ] Rate limiting on auth endpoints?
- [ ] Rate limiting on public endpoints?
- [ ] CORS configured correctly? (not `*` in production)
- [ ] HTTPS enforced?
- [ ] Sensitive data in query params? (should be body/headers)
- [ ] API keys/secrets in responses that shouldn't be?

**Dependencies**
- [ ] Check package.json / requirements.txt for known-vulnerable packages
- [ ] Dev dependencies not included in production build?
- [ ] Lockfile present (package-lock.json / yarn.lock / Pipfile.lock)?

**Environment & Secrets**
- [ ] .env in .gitignore?
- [ ] No secrets committed to repo? (check for API keys, passwords in code)
- [ ] .env.example present with dummy values (not real credentials)?

**Error Handling & Logging**
- [ ] Stack traces not exposed to clients in production?
- [ ] Error messages generic (not revealing internal details)?
- [ ] Sensitive data not logged (passwords, tokens, PII)?

### Severity Classification

**CRITICAL** — Exploitable immediately, direct data breach or account takeover risk
- Hardcoded secrets
- SQL injection with no parameterisation
- Auth bypass
- Exposed admin endpoints

**HIGH** — Significant risk, likely exploitable with moderate effort
- Missing auth on endpoints that should be protected
- No input validation on user-supplied data
- Weak password hashing
- Wildcard CORS in production

**MEDIUM** — Risk exists, harder to exploit or lower impact
- Missing rate limiting
- Verbose error messages
- Missing CSRF protection on state-changing endpoints
- Overly permissive file upload

**LOW** — Best practice violations, low direct risk
- Missing security headers
- Weak session configuration
- Dev tools accessible in production

### Audit Report Format

```
╔══════════════════════════════════════════════════════════╗
║              CAG — SECURITY AUDIT REPORT                 ║
╚══════════════════════════════════════════════════════════╝

📦 REPO: {repo}
🔍 SCOPE: Full codebase audit via CKM + targeted file reads
📅 DATE: {date}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 CRITICAL ({n})
━━━━━━━━━━━
[CRIT-1] {file}:{line-context}
Issue: {description}
Attack: {how an attacker would exploit this}
Fix: {exact code change required}

🔴 HIGH ({n})
━━━━━━━━━━━
[HIGH-1] {file}
Issue: {description}
Fix: {fix}

🟡 MEDIUM ({n})
━━━━━━━━━━━━━
...

🟢 LOW ({n})
━━━━━━━━━━
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ SECURE
{list of things done correctly — always include this section}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 REMEDIATION PRIORITY
1. {CRIT-1 fix} — {effort estimate}
2. {CRIT-2 fix} — {effort estimate}
...
```

---

## Playbook 2 — Code Review

Perform a thorough code review across the codebase.

### Review Dimensions

**Code Quality**
- Dead code / unused imports / unused variables
- Functions that are too long (>50 lines is a warning sign)
- Deeply nested conditionals (>3 levels = refactor candidate)
- Magic numbers/strings without constants
- Repeated code that should be extracted
- Inconsistent naming conventions
- Missing error handling (unhandled promise rejections, missing try/catch)

**Performance**
- N+1 query patterns (loop that queries DB per iteration)
- Missing database indexes (based on query patterns in code)
- Synchronous operations that should be async
- Missing pagination on list endpoints
- Unnecessary data fetching (selecting * when specific columns suffice)
- Memory leaks (event listeners not cleaned up, large objects in closure scope)

**Maintainability**
- Missing tests for core business logic
- Functions with side effects that aren't obvious from their name
- God classes/files doing too many things
- Hard-coded configuration that should be environment variables
- Missing TypeScript types / `any` usage

**Architecture**
- Circular dependencies
- Business logic in route handlers (should be in services)
- Direct DB queries in controllers (should be abstracted)
- Inconsistent patterns across similar features

### Code Review Report Format

```
╔══════════════════════════════════════════════════════════╗
║              CAG — CODE REVIEW REPORT                    ║
╚══════════════════════════════════════════════════════════╝

📦 REPO: {repo}
🔍 FILES REVIEWED: {n}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏆 STRENGTHS
{things done well — specific, not generic praise}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔧 ISSUES BY FILE

📁 {filepath}
  [QUALITY] {issue} — {suggested fix}
  [PERF]    {issue} — {suggested fix}

📁 {filepath}
  ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 METRICS
  Avg function length:    {estimate}
  Test coverage:          {estimate}
  TypeScript strictness:  {strict / loose / none}
  Lint config:            {ESLint / Prettier / none}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 TOP 5 PRIORITIES
1. {highest impact fix}
2. ...
```

---

## Playbook 3 — Feature Planning

User wants to add a new feature. Produce an implementation plan.

### Process

1. Understand the feature request fully
2. Identify all affected areas using the CKM:
   - Schema changes needed (new tables, new columns, new relations)
   - New API endpoints required
   - Existing endpoints to modify
   - Service layer changes
   - Frontend changes (if full-stack repo)
   - Migration strategy
   - Test requirements

### Feature Plan Format

```
╔══════════════════════════════════════════════════════════╗
║         CAG — FEATURE IMPLEMENTATION PLAN                ║
╚══════════════════════════════════════════════════════════╝

📦 REPO: {repo}
✨ FEATURE: {feature name}
⏱️  ESTIMATE: {rough effort — hours / days}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📐 SCHEMA CHANGES
{new tables/collections, new columns, modified relations}

SQL / Prisma / Mongoose diff shown

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🌐 API CHANGES

New endpoints:
  POST /api/{resource}     — {description}
  GET  /api/{resource}/:id — {description}

Modified endpoints:
  GET /api/users — add {field} to response

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 FILES TO CREATE / MODIFY

CREATE:
  src/services/{feature}.ts     — Core business logic
  src/routes/{feature}.ts       — Route handlers
  src/models/{feature}.ts       — Data model (if new entity)
  tests/{feature}.test.ts       — Test suite

MODIFY:
  prisma/schema.prisma          — Add {model}
  src/routes/index.ts           — Register new router
  src/types/index.ts            — Add {Type}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔧 IMPLEMENTATION ORDER

1. {step 1 — schema/migration}
2. {step 2 — model/service}
3. {step 3 — routes/handlers}
4. {step 4 — tests}
5. {step 5 — any frontend changes}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ WATCH OUT FOR
{gotchas specific to this codebase that affect this feature}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 STARTER CODE

{generate boilerplate for the new service and route files,
 following the exact patterns used in the existing codebase}
```

---

## Playbook 4 — Refactor Plan

User wants to refactor part of the codebase.

### Process

1. Understand what they want refactored and why
2. Map all dependencies of the target code using CKM
3. Identify risk areas (what breaks if this changes?)
4. Produce before/after with migration path

### Output Format

```
╔══════════════════════════════════════════════════════════╗
║             CAG — REFACTOR PLAN                          ║
╚══════════════════════════════════════════════════════════╝

📦 TARGET: {file or module being refactored}
🎯 GOAL: {what the refactor achieves}
⚠️  RISK: {High / Medium / Low} — {reason}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔗 DEPENDENCY MAP
Files that import {target}:
  {list of callers — from CKM dependency_graph}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 BEFORE / AFTER

BEFORE: {current pattern}
AFTER:  {proposed pattern}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🪜 MIGRATION STEPS (safe, incremental order)

1. {step — write new version alongside old}
2. {step — migrate callers one by one}
3. {step — delete old version}
4. {step — update tests}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ TEST COVERAGE NEEDED
{what tests to write/update to safely do this refactor}
```

---

## Playbook 5 — Performance Analysis

Identify performance bottlenecks from the codebase.

### Signals to Look For

**Database**
- N+1 queries: `for (const item of items) { await db.query(...) }` pattern
- Missing `.select()` — fetching all columns when only some needed
- Missing pagination on list queries
- Missing indexes (cross-reference query patterns with schema)
- Large transactions that could be broken up

**API**
- Synchronous operations blocking the event loop (Node.js)
- Missing caching on expensive repeated queries
- Large response payloads (missing field selection / projection)
- No compression (gzip/brotli) configured

**Frontend (if full-stack)**
- Waterfall data fetching (sequential awaits that could be parallel)
- Missing memoisation on expensive computations
- Large bundle size signals (importing entire libraries for one function)

### Output Format

```
╔══════════════════════════════════════════════════════════╗
║          CAG — PERFORMANCE ANALYSIS                      ║
╚══════════════════════════════════════════════════════════╝

📦 REPO: {repo}

🐌 BOTTLENECKS DETECTED

[CRITICAL] N+1 query in {file}:{function}
  Pattern: {code snippet}
  Fix: Use {include/eager loading/DataLoader} — example:
  {fixed code}

[HIGH] Missing index for {query pattern} in {model}
  Query at: {file}
  Fix: Add index on {field(s)}
  SQL: CREATE INDEX idx_{table}_{field} ON {table}({field});

[MEDIUM] Sequential async calls in {file}
  Fix: Use Promise.all([...])

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚡ QUICK WINS (low effort, high impact)
{list 3-5 changes that are easy to make and meaningfully improve perf}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 ESTIMATED IMPACT
{rough estimate of what fixing each issue achieves}
```

---

## General Query Rules

- **Always cite sources** — `(src/services/auth.ts#47)` style references
- **Show the actual code** — don't describe what code does without showing it
- **Make recommendations actionable** — every issue gets a concrete fix
- **Rank by impact** — most important changes first, always
- **Acknowledge limits** — if you haven't read a file, say so; offer to fetch it

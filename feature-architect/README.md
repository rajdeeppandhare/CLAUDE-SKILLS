# 🏗️ Feature Architect

> **Turn feature requests into production-ready implementation blueprints — before a single line of code is written.**

Most AI coding tools are excellent at *writing code*. Feature Architect is different. It thinks the way a Staff Engineer thinks: analyse the existing system, map the blast radius, design the architecture, surface the risks — then produce a step-by-step blueprint tailored to *your* project. Code comes last.

---

## ✨ What It Does

| Input | Output |
|---|---|
| GitHub repo URL + feature request | Full project context analysis |
| Pasted code + "add X" | Impact map (files, DB, APIs, auth) |
| Tech stack description + feature idea | Architecture design (project-specific) |
| Multi-feature request | Risk report with severity ratings |
| | Numbered implementation blueprint |
| | Sequenced step-by-step code plan |
| | Optional: actual generated code |

---

## 🆚 The Problem with Regular AI Coding

```
You:  "Add Stripe payments"
AI:   Here is the code...
```

The AI doesn't know your folder structure, your auth flow, your existing subscription logic, or your database schema. The code works in isolation and breaks in your project. Then you spend hours debugging integration issues that a senior engineer would have caught in 10 minutes of design thinking.

Feature Architect solves this.

---

## 🔄 The 6-Phase Pipeline

```
PHASE 1 — PROJECT SCAN       Reads your actual codebase
PHASE 2 — IMPACT MAPPING     Identifies every file, table, and service touched
PHASE 3 — ARCHITECTURE       Designs the feature for your specific stack
PHASE 4 — RISK DETECTION     Surfaces hidden problems before you hit them
PHASE 5 — BLUEPRINT          Step-by-step implementation roadmap
PHASE 6 — CODE GENERATION    Actual code, only after design is locked
```

---

## 📦 Example Output

```
╔══════════════════════════════════════════════════════════════╗
║          FEATURE ARCHITECT — IMPLEMENTATION BLUEPRINT        ║
╚══════════════════════════════════════════════════════════════╝

🎯 FEATURE:     Stripe Subscription Billing
📦 PROJECT:     acme-saas / Next.js 14 + Prisma + PostgreSQL
📊 COMPLEXITY:  High
⏱️  ESTIMATE:    12–15 hours
📁 FILES:       8 new, 6 modified
🗄️  DB CHANGES:  3 tables, 2 migrations
⚡ BREAK RISK:  Medium (auth middleware changes required)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 RISKS DETECTED (3 issues)

⚠️ RISK 1: Auth roles don't support subscription tiers
   Severity: HIGH | Your current auth has boolean isAdmin only.
   Mitigation: Add subscription_tier enum to users table first.

⚠️ RISK 2: Webhook retries not idempotent
   Severity: HIGH | Stripe retries failed webhooks up to 3x.
   Mitigation: Add idempotency_key column to billing_events.

⚠️ RISK 3: Race condition on upgrade flow
   Severity: MEDIUM | Concurrent upgrade + invoice requests.
   Mitigation: Use DB transaction wrapping subscription update.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 IMPLEMENTATION STEPS

STEP 1 — Extend Auth Schema
  Files:    prisma/schema.prisma (modify), migration
  Action:   Add subscription_tier enum + plan fields to User model
  ✓ Done when: Migration runs clean, existing users default to 'free'

STEP 2 — Create Billing Service
  Files:    src/services/billing.ts (create)
  Action:   Stripe SDK wrapper — createCustomer, createSubscription,
            cancelSubscription, getInvoices
  ✓ Done when: Unit tests pass for all four methods

STEP 3 — Webhook Handler
  Files:    app/api/webhooks/stripe/route.ts (create)
  Action:   Handle checkout.session.completed, customer.subscription.updated,
            invoice.payment_failed with idempotency check
  ✓ Done when: Stripe CLI test delivers all event types cleanly

[...continues for all 8 steps]
```

---

## 🚨 Risk Detection

The most valuable phase. Feature Architect catches what developers miss:

| Risk Type | Example |
|---|---|
| **Auth gaps** | Your auth has no role system — subscription tiers need one |
| **Race conditions** | Two upgrade requests hitting the same record |
| **Webhook idempotency** | Stripe retrying the same payment event three times |
| **Breaking API changes** | Modifying `/api/users` response shape breaks mobile clients |
| **Migration safety** | Adding NOT NULL column with no default to a live table |
| **N+1 queries** | Fetching team members inside a loop for every notification |
| **Missing indexes** | Querying `notifications` by `user_id` with no index |
| **Dependency conflicts** | New package requires Node 20, project runs Node 18 |

---

## 📊 Multi-Feature Mode

Planning several features at once? Feature Architect produces a combined complexity report and recommends build order:

```
MULTI-FEATURE COMPLEXITY REPORT

Feature          Complexity   Estimate   Break Risk
─────────────────────────────────────────────────
Team Sharing     High         10–12h     Medium
Notifications    Medium        4–6h      Low
Stripe Billing   High         12–15h     Medium

COMBINED: 26–33h | Recommended order: Notifications → Team → Billing
Reason: Billing modifies auth middleware that Team Sharing also needs
```

---

## 🎯 Who This Is For

- **Solo founders** — ship features correctly the first time, not after painful refactors
- **Junior developers** — get a Staff Engineer's design review before touching code
- **Senior developers** — accelerate architecture planning for complex integrations
- **Non-technical founders** — understand exactly what "add feature X" actually means for your product
- **Teams** — use blueprints as implementation tickets before handing to developers

---

## 🚀 Usage

**Option 1 — GitHub URL + Feature**
```
Architect this feature for my project:
https://github.com/yourusername/your-app

I want to add real-time notifications using WebSockets.
```

**Option 2 — Paste Code + Feature**
```
Here are my main files: [paste code]

I want to add Stripe billing with monthly and annual plans.
```

**Option 3 — Stack Description + Feature**
```
I'm building a Next.js 14 app with Prisma + PostgreSQL and JWT auth.
I want to add team collaboration — multiple users per organisation.
```

**Option 4 — Multi-Feature**
```
I want to plan three features before I start building:
1. Real-time notifications
2. Team workspaces
3. Stripe billing

Analyse all three and tell me what order to build them in.
```

---

## 🏆 Features Supported

Feature Architect has deep blueprints for the most common integrations:

| Category | Features |
|---|---|
| **Payments** | Stripe subscriptions, one-time payments, invoicing, trials |
| **Auth & Identity** | OAuth (Google/GitHub), SSO, RBAC, magic links, 2FA |
| **Real-time** | WebSockets, SSE, push notifications, live updates |
| **Collaboration** | Teams, organisations, member roles, invitations |
| **Search** | Full-text search, Algolia, Typesense, Elasticsearch |
| **File Handling** | S3 uploads, image processing, CDN, signed URLs |
| **Email** | Transactional email, queues, templates, bounce handling |
| **AI Features** | LLM integration, embeddings, vector search, streaming |
| **Analytics** | Event tracking, funnels, dashboards, data pipelines |
| **Admin** | Admin panels, audit logs, feature flags, impersonation |

---

## 📚 References

- [Martin Fowler — Patterns of Enterprise Application Architecture](https://martinfowler.com/books/eaa.html)
- [Stripe Integration Best Practices](https://stripe.com/docs/best-practices)
- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [The Twelve-Factor App](https://12factor.net/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — Security risks to design against

---

## 🗂️ Skill Structure

```
feature-architect/
├── SKILL.md                          ← Core pipeline logic
├── README.md                         ← This file
└── references/
    ├── risk-patterns.md              ← Deep risk pattern library (80+ patterns)
    └── feature-blueprints.md         ← Pre-built blueprints for common features
```

---

*Part of the [Claude Skills Portfolio](https://github.com/yourusername/claude-skills) — production-grade skills for Claude.ai.*

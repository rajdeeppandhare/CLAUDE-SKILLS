# Stack Evolution Engine

> Engineering-grade migration blueprints for any stack transformation.

---

## What This Skill Does

Stack Evolution Engine turns a GitHub repo + a target stack into a complete, production-safe migration plan. It doesn't produce generic advice — it reads your actual codebase, scores the migration across five dimensions, makes an honest GO / NO-GO recommendation, and delivers a phased blueprint with rollback strategies and a PR roadmap.

---

## When It Triggers

Load this skill whenever a user:

- Shares a GitHub URL alongside a target technology
- Asks "should I migrate to X?", "is it worth moving to Y?", or "what would it take to switch to Z?"
- Uses phrases like *migrate*, *convert*, *move from*, *switch to*, *rewrite in*, *evolve my stack*, *modernise*, or *upgrade my architecture*
- Pastes code and asks about stack migration without a GitHub link

---

## The Five-Phase Pipeline

| Phase | Name | Output |
|---|---|---|
| 1 | Scan | Evolution Profile — language, framework, DB, auth, deps, test coverage |
| 2 | Assess | Complexity / Risk / Feasibility / Value scores + effort estimate |
| 3 | Decide | GO / PARTIAL GO / NO-GO with honest rationale |
| 4 | Blueprint | Phased migration plan, tech mappings, breaking points, rollback strategy, PR roadmap |
| 5 | Deliver | Formatted Evolution Report + exportable artefacts |

---

## Reference Files

| File | Purpose | Load When |
|---|---|---|
| `references/stack-mappings.md` | Detailed source → target guidance for 15+ common migrations | User names a specific stack pair |
| `references/evolution-patterns.md` | Deep-dive on Strangler Fig, Parallel Run, Branch by Abstraction, etc. | User asks about migration strategy or patterns |
| `references/ticket-templates.md` | JIRA / Linear / GitHub Issues ticket templates with acceptance criteria | User asks for tickets or sprint planning |
| `references/no-migration-alternatives.md` | What to do instead when migration is the wrong call | Decision is NO-GO or PARTIAL GO |

---

## Supported Migration Types

**Languages**
- JavaScript → TypeScript
- Python → Go
- Java → Kotlin
- Python → Rust

**Frameworks**
- React → Next.js
- Express → Fastify
- Flask / FastAPI → Go (Fiber / Chi / Gin)
- Django → FastAPI
- Angular → React

**Databases**
- MongoDB → PostgreSQL
- MySQL → PostgreSQL
- Firebase Firestore → Supabase
- SQLite → PostgreSQL

**Architecture**
- Monolith → Microservices
- REST → GraphQL
- Synchronous → Event-Driven

**Infrastructure**
- Firebase → Supabase (full platform)
- Docker Compose → Kubernetes
- AWS → GCP

---

## Output Example

```
╔══════════════════════════════════════════════════════════════╗
║              STACK EVOLUTION ENGINE — REPORT                 ║
╚══════════════════════════════════════════════════════════════╝

📦 REPO:       acme/api-server
🔁 EVOLUTION:  Express + JS → Fastify + TypeScript
📅 GENERATED:  2025-06-15

⚡ VERDICT

  Decision:          GO
  Rationale:         The codebase is 142 files with medium test coverage.
                     TypeScript adds type safety with near-zero breaking risk.
                     Fastify gains 2–3× throughput for the price of a framework swap.

  Complexity:        4/10
  Risk:              3/10
  Value:             8/10
  Feasibility:       9/10

  Estimated Effort:  78h (~3 weeks for 2 engineers)
  Migration Pattern: Layer-by-Layer
```

---

## Guardrails

- Never recommend migration without reading the actual repo.
- NO-GO is a valid and valuable output — push back on bad migrations.
- Never hallucinate file paths, function names, or dependencies.
- Always surface the 20% buffer in effort estimates — engineers underestimate migrations by 3–5×.
- Always prioritise zero-downtime approaches. Flag any phase requiring a maintenance window.

---

## Folder Structure

```
stack-evolution/
├── SKILL.md                          ← Main skill prompt
├── .skill                            ← Skill metadata
├── README.md                         ← This file
└── references/
    ├── stack-mappings.md             ← Source → target migration guides
    ├── evolution-patterns.md         ← Migration pattern deep-dives
    ├── ticket-templates.md           ← JIRA / Linear / GitHub ticket templates
    └── no-migration-alternatives.md  ← Alternatives when migration is the wrong call
```

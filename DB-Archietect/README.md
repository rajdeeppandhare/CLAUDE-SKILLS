# 🧠 Context Memory — Claude Skill

> A portable conversation memory skill for Claude that snapshots your entire session into a downloadable file — so you can resume exactly where you left off in any new chat, or even on a different model.

---

## ✨ What It Does# 🗄️ DB Architect

> **Drop your GitHub repo. Get your entire database — analysed, designed, schemed, and wired into your code.**

DB Architect reads your actual codebase and produces two things: a full architectural decision report, and a ready-to-execute schema package that integrates directly with your existing code. No vague advice. No generic tutorials. Real output for your real project.

---

## ✨ What It Does

| Input | Output |
|---|---|
| GitHub repo URL | DB recommendation with confidence score + reasoning |
| Pasted code files | Full schema (SQL DDL / Prisma / Mongoose / Firestore) |
| Uploaded project files | File-by-file code change map |
| | Connection boilerplate (copy-paste ready) |
| | `.env` template with every required variable |
| | Step-by-step `SETUP.md` — provision → connect → migrate → verify |

---

## 🏆 Databases Covered

| Database | Best For |
|---|---|
| **PostgreSQL** | Relational data, SaaS, fintech, complex queries |
| **MongoDB Atlas** | Flexible schemas, content platforms, nested docs |
| **Firebase Firestore** | Real-time sync, mobile apps, full BaaS |
| **Supabase** | Postgres + auth + storage + realtime in one |
| **Neon** | Serverless Postgres, edge deployments |
| **PlanetScale** | Serverless MySQL, schema branching |
| **Redis** | Caching, sessions, rate limiting (alongside primary DB) |
| **TimescaleDB** | Time-series, IoT, analytics on Postgres |
| **ClickHouse** | OLAP, analytics, event pipelines |

---

## 🔍 How the Analysis Works

DB Architect reads your code across five dimensions before making a single recommendation:

**Data Shape** — Nested/hierarchical entities signal document stores. FK-heavy relational patterns signal SQL. Polymorphic associations get flagged explicitly.

**Query Patterns** — Filter-heavy routes, aggregation pipelines, full-text search needs, real-time requirements — all pulled from your actual route and controller files.

**Scale Signals** — MVP vs production vs enterprise. Serverless vs persistent server. Existing cloud provider. All affect the recommendation.

**Stack Signals** — Your framework and language determine which ORMs are available. Next.js + TypeScript → Prisma path. FastAPI → SQLAlchemy. Laravel → Eloquent. The skill knows the ecosystem.

**Existing Config** — If you already have a DB partially wired up, the skill integrates and extends — it doesn't rip out and replace.

Then it makes a **firm, reasoned recommendation** — not a "here are 5 options, you decide" non-answer.

---

## 📦 Example Output

```
╔══════════════════════════════════════════════════════╗
║           DB ARCHITECT — ANALYSIS REPORT             ║
╚══════════════════════════════════════════════════════╝

📦 REPO: acme-saas-app
🔍 STACK DETECTED: Next.js 14, TypeScript, Prisma (no datasource configured)
📊 DATA COMPLEXITY: Moderate
⚡ SCALE ESTIMATE: MVP → Growth

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏆 RECOMMENDATION: PostgreSQL via Supabase
   Confidence: High | Why not MongoDB: 6 entity types with clear FK
   relationships — would fight document model the whole way

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📐 SCHEMA OVERVIEW
→ 7 tables: users, organisations, org_members, projects, tasks, comments, files
→ Row-level security policies for multi-tenant isolation
→ 12 indexes recommended based on query patterns found in routes/

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔧 CODE CHANGES REQUIRED
📁 prisma/schema.prisma     → Full schema (datasource + all 7 models)
📁 lib/prisma.ts            → Create singleton (missing entirely)
📁 .env.example             → Add DATABASE_URL + DIRECT_URL
📁 app/api/projects/route   → 3 raw fetch() calls → Prisma client queries
📁 app/api/tasks/route      → Missing pagination → add cursor-based
```

---

## 📄 Execution Package

Every project gets a complete set of copy-paste-ready files:

```
schema.sql                  ← Run in Supabase SQL editor or psql
prisma/schema.prisma        ← Full Prisma schema, all models + relations
lib/db.ts                   ← Connection singleton with pooling configured
.env.example                ← Every required environment variable
SETUP.md                    ← Full setup walkthrough, step by step
```

No `// TODO` placeholders. No "add your logic here." If a variable needs a value you supply, it shows the exact format.

---

## 🚀 Usage

**Option 1 — GitHub URL**
```
Analyse this repo and design the database:
https://github.com/yourusername/your-project
```

**Option 2 — Paste Code**
```
Here are my model files and routes. Design the database for this:
[paste your code]
```

**Option 3 — Upload Files**
Upload your project files directly and ask:
```
Build the database architecture for this project
```

---

## 🎯 Who This Is For

- **Indie hackers & solo founders** — you've built the UI, now you need the data layer done right
- **Bootcamp grads** — you know how to code but DB architecture is genuinely hard to get right
- **Junior developers** — get a senior architect's opinion on your specific codebase, not Stack Overflow generics
- **Startup teams** — settle the Mongo vs Postgres debate with real analysis, not vibes

---

## 📚 References

- [PostgreSQL Documentation](https://www.postgresql.org/docs/) — official Postgres docs
- [Prisma Schema Reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference) — Prisma ORM
- [MongoDB Atlas Docs](https://www.mongodb.com/docs/atlas/) — MongoDB managed cloud
- [Supabase Docs](https://supabase.com/docs) — Supabase platform
- [Firebase Firestore](https://firebase.google.com/docs/firestore) — Firestore data model
- [Use The Index, Luke](https://use-the-index-luke.com/) — SQL indexing best practices
- [Neon Docs](https://neon.tech/docs) — Serverless Postgres
- [Drizzle ORM](https://orm.drizzle.team/docs/overview) — lightweight TypeScript ORM

---

## 🗂️ Skill Structure

```
db-architect/
├── SKILL.md                                   ← Core skill logic & pipeline
├── README.md                                  ← This file
└── references/
    ├── db-comparison-deep-dive.md             ← Full DB tradeoffs, strengths, pricing
    ├── schema-patterns-library.md             ← Battle-tested schemas for common domains
    ├── orm-integration-guide.md               ← ORM setup per language & framework
    ├── performance-and-indexing.md            ← Query optimisation, indexes, N+1, pooling
    └── cloud-provisioning-guide.md            ← How to provision each DB on each cloud
```

---

*Part of the [Claude Skills Portfolio](https://github.com/yourusername/claude-skills) — production-grade skills for Claude.ai.*

Hit your token limit? Switching models? Want to save your progress? Context Memory builds a rich **Memory Map** of your conversation that captures everything needed to resume seamlessly:

- 🗺️ **Maps** your entire session — goals, decisions, code, issues, dead ends
- 📥 **Exports** a clean Markdown file you can download anytime
- 🔄 **Resumes** instantly — upload the file to any new chat and pick up mid-sentence
- 🤖 **Cross-model** — works with any Claude instance, or any AI that can read Markdown
- ⚡ **Proactive** — Claude can offer a checkpoint automatically when sessions get long

---

## 📋 What Gets Captured

| Section | What's Inside |
|---|---|
| 🎯 Session Goal | What you were trying to build or solve |
| 📍 Current Status | Exact state + next planned step |
| ✅ Accomplishments | Every completed step, in order |
| 🐛 Issues & Resolutions | Problems found, fixed, or still open |
| 💻 Code & Artifacts | All files produced, with full content |
| 🔑 Key Decisions | Why you chose X over Y |
| ⚠️ Dead Ends | What failed — so you don't repeat it |
| 📦 Environment | OS, language, deps, env vars, paths |
| 💬 Chat Highlights | The exchanges that actually mattered |
| 🚀 Resume Instructions | Exact prompt to hand to the next Claude |

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `context-memory.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `context-memory/` folder
3. Rename to `context-memory.skill`
4. Install via Claude.ai **Settings → Skills**

---

## 🚀 How to Use

### Saving a Memory Map
Once installed, just ask naturally:

```
"Save my progress"
"Export this conversation"
"I'm running out of tokens — checkpoint this"
"Create a memory map"
"I want to continue this in a new chat"
```

Claude will generate and export a `memory-map-*.md` file you can download.

### Resuming in a New Chat
1. Start a fresh chat
2. Upload your `memory-map-*.md` file
3. Say:

```
"Resume from my memory map"
```

Claude will read the file, confirm the current state, and pick up exactly where you left off — no re-explaining needed.

---

## 📊 Example Output

```markdown
# 🧠 Conversation Memory Map
**Exported:** 2026-05-31
**Model:** Claude Sonnet 4.6

## 🎯 Session Goal
Build a JWT authentication system for a Node.js/Express API with
refresh token rotation and Redis session storage.

## 📍 Current Status
**Where we are:** Auth middleware is working; refresh token endpoint returns 200
**Next planned step:** Implement token blacklisting on logout
**Confidence level:** ~65% complete

## ✅ What Was Accomplished
1. Set up Express server with basic routing
2. Implemented /login endpoint with bcrypt password check
3. JWT access token generation working (15min expiry)
4. Refresh token stored in Redis with 7-day TTL
5. Auth middleware verified on protected routes

## ⚠️ Dead Ends & Things That Failed
- Tried jsonwebtoken v9 → broke with ESM imports, downgraded to v8.5.1
- Redis connection with `ioredis` default config failed on WSL2 →
  needed explicit host: '127.0.0.1' instead of 'localhost'
...
```

---

## 🔄 Resume Mode

When you upload a Memory Map to a new chat, Claude automatically switches into **Resume Mode**:

```
## 🔄 Session Resumed

Goal: Build JWT auth system for Node.js/Express API
Picking up at: Implement token blacklisting on logout
Tracking these open issues:
  - Redis on WSL2 needs explicit 127.0.0.1 host ⚠️

Stack: Node.js 20, Express 4, Redis via ioredis, WSL2

Is anything different from when this was saved?
Otherwise I'll jump straight into the blacklisting logic now.
```

---

## 💡 Proactive Checkpoints

You don't have to ask — Claude will offer automatically when:

- The conversation gets long and complex
- A major milestone is reached
- You mention running low on tokens
- You say you're stepping away and coming back later

---

## 📁 Skill Structure

```
context-memory/
├── SKILL.md                      # Core skill logic & memory map schema
├── README.md                     # This file
├── context-memory.skill          # Installable skill package
└── references/
    ├── memory-map-schema.md      # Detailed schema guide for every section
    └── resume-mode-patterns.md   # Resume mode behaviour & edge cases
```

---

## 🧠 Built With

- [Claude Skills](https://claude.ai) — Anthropic's skill system for extending Claude
- Portable Markdown format — readable by any AI or human

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built as a portfolio project demonstrating Claude skill development — solving the real problem of lost context across long AI sessions.*

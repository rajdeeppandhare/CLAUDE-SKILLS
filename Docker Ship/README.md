# CAG — Claude-Augmented Generation

> Drop a GitHub URL. Get a senior engineer's understanding of the entire codebase.

CAG is a Claude skill that performs a complete codebase ingestion pipeline — crawl, parse, map, and index any GitHub repository into a structured in-context knowledge base. Then ask it anything.

---

## What It Does

```
You:    https://github.com/vercel/next.js
Claude: [crawls 4,200 files, builds knowledge map, generates architecture diagram]
        ✅ CAG READY — Ask me anything about this codebase.

You:    How does the App Router handle server components?
Claude: [traces the exact code path through next/src/server/app-router.ts → ...]
```

CAG doesn't just read a README — it understands the actual code. It traces real function calls, maps real dependencies, detects real bugs.

---

## Features

**Full Codebase Ingestion**
Crawls the GitHub file tree recursively, fetches files in priority order (entry points → stack config → architecture core → deep logic), and builds a complete Codebase Knowledge Map (CKM) from what it reads.

**Structured Analysis Report**
After ingestion, produces a formatted report covering: detected stack, architecture diagram, module map, data model, full API surface, and issues detected — all from real code, not inference.

**Portable Snapshots**
Exports a `.cag` snapshot file after every ingestion. Upload it to any future Claude session to instantly restore full codebase context — no re-fetching, no re-crawling.

**Query Mode**
Once loaded, answer any question: how does X work, find security vulnerabilities, generate a README, write an onboarding guide, plan a new feature, audit for performance issues.

**Documentation Generation**
Generates README, API reference docs, architecture documents, onboarding guides, and per-module documentation — all from actual code, never templated filler.

---

## Skill Structure

```
cag/
├── SKILL.md                    # Core skill logic — the CAG pipeline
├── README.md                   # This file
├── cag.skill                   # Installable skill package
└── references/
    ├── ckm-schema.md           # CKM data model, snapshot format, doc templates
    └── query-playbook.md       # Security audit, code review, refactor, feature planning
```

---

## Usage

### 1. Ingest a Repository

Paste any public GitHub URL:
```
https://github.com/owner/repo
```

CAG will:
1. Crawl the full file tree via GitHub API
2. Ingest files in priority order
3. Build the Codebase Knowledge Map
4. Output the Analysis Report
5. Export a `.cag` snapshot file
6. Enter Query Mode

### 2. Resume from a Snapshot

Upload a `.cag` file and say:
```
load CAG snapshot
```

Context is restored instantly. No re-fetching required.

### 3. Query Examples

```
"How does authentication work?"
"Walk me through the data flow for checkout"
"Find security vulnerabilities"
"What would I need to add Stripe payments?"
"Generate a full README"
"Onboard me to this project"
"Explain the architecture"
"Review the codebase for performance issues"
"Which parts have no test coverage?"
```

---

## What CAG Produces

### Analysis Report
```
╔══════════════════════════════════════════════════════════╗
║              CAG — CODEBASE ANALYSIS REPORT              ║
╚══════════════════════════════════════════════════════════╝

📦 REPO:     vercel/commerce
🌐 URL:      https://github.com/vercel/commerce
⭐ STARS:    12.4k | 🍴 FORKS: 3.1k
📝 ABOUT:    Next.js Commerce — high-performance e-commerce starter

🔍 STACK DETECTED
   Language:   TypeScript
   Framework:  Next.js 14 (App Router)
   Database:   Shopify Storefront API
   Auth:       No auth (storefront only)
   Deployment: Vercel
   Testing:    None detected

🏗️ ARCHITECTURE
   Type:    Monolith
   Pattern: Feature-based

   ┌─────────────┐
   │   Browser   │
   └──────┬──────┘
          │
   ┌──────▼──────┐
   │  Next.js    │  App Router + RSC
   │  app/       │
   └──────┬──────┘
          │
   ┌──────▼──────┐
   │  lib/       │  Shopify API client
   └──────┬──────┘
          │
   ┌──────▼──────┐
   │  Shopify    │  Storefront GraphQL API
   │  Storefront │
   └─────────────┘

...
✅ CAG READY — Ask me anything about this codebase.
```

---

## Limitations

**Context window**: CAG can fully ingest repos up to ~150 files. For larger repos, it fully reads Tier 1–2 files (config, entry points, stack fingerprint) and uses directory structure to infer the rest. It is always transparent about what was directly read vs inferred.

**Private repos**: CAG will prompt you to either paste key files directly or provide a GitHub personal access token.

**Snapshot freshness**: `.cag` snapshots older than 30 days will trigger a warning. Re-run CAG on the repo URL to refresh.

**No live execution**: CAG reads and understands code — it doesn't run it. Performance analysis and bug detection are static analysis only.

---

## How It Works

CAG uses Claude's context window as a knowledge base. Unlike traditional RAG (Retrieval-Augmented Generation) which embeds text into external vector stores, CAG:

1. Fetches code directly from the GitHub API
2. Builds a structured Codebase Knowledge Map (CKM) in-context
3. Uses the CKM to answer questions with full source traceability

No external services. No embeddings. No vector databases. Just Claude reading real code and reasoning about it.

The `.cag` snapshot format makes this persistent — the CKM can be serialised to Markdown and re-loaded in any session.

---

## Built With

- Claude (claude.ai skills)
- GitHub REST API (file tree + raw content)
- No external dependencies

---

*A Claude skill by Raja — part of the CAG codebase intelligence toolkit.*

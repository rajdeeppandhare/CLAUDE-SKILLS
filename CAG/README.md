# CAG — Claude-Augmented Generation

> Chat with any GitHub codebase like a senior engineer who's been on the project for months.

CAG is a Claude skill that ingests an entire GitHub repository, builds a structured in-context knowledge map, and then answers any question about it with precision — tracing real code paths, citing actual file names, and generating accurate documentation without hallucination.

---

## What It Does

Point CAG at a GitHub repo URL. It crawls the full file tree, ingests the priority files, builds a **Codebase Knowledge Map (CKM)**, and exports a portable `.cag` snapshot. From there you can ask anything: architecture questions, security audits, feature planning, onboarding guides, full READMEs — all grounded in real code.

---

## Features

- **Full repo ingestion** — crawls the GitHub tree recursively, fetches files in priority order (identity → stack → architecture → deep logic)
- **Codebase Knowledge Map** — structured in-memory index of modules, data model, API surface, dependency graph, and detected issues
- **`.cag` snapshot export** — portable Markdown file you can upload to any Claude session to restore full context instantly, no re-fetching
- **Query mode** — answer any question with exact file paths, function names, and code traces
- **Documentation generation** — README, API reference, architecture docs, onboarding guides, per-module docs (via `doc-templates.md`)
- **Security audit** — systematic sweep across auth, authorisation, input validation, file handling, API security, secrets, and deps (via `query-playbook.md`)
- **Code review** — quality, performance, maintainability, and architecture issues ranked by impact
- **Feature planning** — schema changes, new endpoints, files to create/modify, implementation order, and boilerplate — all matching the repo's existing patterns
- **Refactor planning** — dependency mapping, before/after, safe incremental migration steps
- **Performance analysis** — N+1 queries, missing indexes, sequential async, bundle bloat, and more

---

## Skill Files

```
cag/
├── SKILL.md                          ← Core skill: pipeline, CKM schema, output format
└── references/
    ├── doc-templates.md              ← Templates: README, API docs, architecture, onboarding, module docs
    └── query-playbook.md             ← Playbooks: security audit, code review, feature plan, refactor, perf analysis
```

`SKILL.md` is the entry point. The two reference files are loaded on demand — Claude pulls them in only when the relevant query type is detected, keeping context usage lean.

---

## How to Use

### 1. Start a new Claude conversation and attach all three skill files

Upload `SKILL.md`, `references/doc-templates.md`, and `references/query-playbook.md` to the conversation, or paste their contents.

### 2. Give Claude a GitHub repo URL

```
Analyse this repo: https://github.com/owner/repo
```

Claude will run the full CAG pipeline (Phases 1–4) and produce an analysis report.

### 3. Ask anything

```
How does authentication work?
Find security vulnerabilities.
Generate a full README.
What would I need to change to add Stripe payments?
Onboard me to this codebase.
Run a performance analysis.
```

### 4. Save and restore with `.cag` snapshots

After analysis, Claude exports a `{repo-name}.cag` file. In a future session, upload it and say:

```
Load CAG snapshot
```

Full context restored instantly — no re-fetching required.

---

## Trigger Phrases

CAG activates on phrases like:

- "Analyse this GitHub repo"
- "Chat with my codebase"
- "Understand this repo"
- "Explain how this project works"
- "Document this codebase"
- "Find bugs in this project"
- "Give me an architecture overview"
- "Index this repo"
- "CAG this repo"

Or any time a GitHub link is shared alongside a request for deep codebase understanding.

---

## Private Repos

If the repo returns a 403 or 404, CAG will prompt you to either paste key files directly or provide a GitHub personal access token:

```
Authorization: token ghp_xxxxxxxxxxxx
```

---

## Guardrails

- **No hallucinated paths or function names** — CAG only references files it has actually fetched
- **Always cites sources** — every answer includes the exact file and context it came from
- **Transparent about gaps** — if a file hasn't been read, Claude says so and offers to fetch it on demand
- **Large repo handling** — repos with 1000+ files use directory structure inference for Tier 3–4 files; inferred vs read content is always distinguished
- **Snapshot freshness** — snapshots older than 30 days trigger a warning and an offer to refresh

---

## Reference Files

| File | Loaded When |
|---|---|
| `references/doc-templates.md` | User asks for README, API reference, architecture docs, onboarding guide, or module docs |
| `references/query-playbook.md` | User asks for code review, security audit, refactor plan, feature planning, or performance analysis |

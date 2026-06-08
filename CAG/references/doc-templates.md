# CAG — Documentation Templates

Reference file for generating documentation from a loaded Codebase Knowledge Map (CKM).
Load this file when the user asks for: README generation, API docs, architecture docs,
onboarding guides, or any documentation output.

---

## Template 1 — README Generator

Generate a full, polished README based on the CKM. Structure:

```markdown
# {project-name}

> {one-line description from repo metadata or README}

{2-3 sentence project summary — what it does, who it's for, what problem it solves}

## ✨ Features
{extract from codebase — list actual implemented features, not aspirational ones}

## 🛠️ Tech Stack
| Layer | Technology |
|---|---|
| Frontend | {framework} |
| Backend | {framework} |
| Database | {db} |
| Auth | {auth method} |
| Deployment | {deploy target} |
| Testing | {framework} |

## 🚀 Quick Start

### Prerequisites
{list from package.json / requirements.txt / go.mod}

### Installation
```bash
git clone {repo_url}
cd {repo_name}
{install command — npm install / pip install / go mod download}
cp .env.example .env
{any seed/migrate commands detected}
{start command — npm run dev / python main.py / go run .}
```

### Environment Variables
| Variable | Description | Required |
|---|---|---|
{extract from .env.example — one row per variable}

## 📁 Project Structure
```
{repo_name}/
{generate actual directory tree from CKM — only top 2 levels + key files}
```

## 🌐 API Reference
{if API surface detected — include endpoint table}

| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
{from api_surface in CKM}

## 🗄️ Data Model
{entity list and key relationships from CKM}

## 🧪 Testing
```bash
{test command from package.json scripts or detected test runner}
```

## 🤝 Contributing
{if CONTRIBUTING.md exists — summarise; otherwise generate standard template}

## 📄 License
{from LICENSE file}
```

**Rules for README generation:**
- Only include sections you have actual data for from the CKM
- Never invent features not found in the code
- Use actual commands from package.json scripts / Makefile / detected tooling
- If a section can't be filled accurately, omit it rather than placeholder it

---

## Template 2 — API Reference Docs

Generate a structured API reference from detected endpoints:

```markdown
# {project-name} API Reference

Base URL: `{base_url if detectable, else http://localhost:{port}}`
Authentication: {auth method — Bearer token / API key / Session cookie}

---

## Authentication

### POST /api/auth/login
{description inferred from handler code}

**Request Body**
```json
{schema inferred from handler validation code}
```

**Response — 200 OK**
```json
{schema inferred from handler response code}
```

**Response — 401 Unauthorized**
```json
{ "error": "Invalid credentials" }
```

---

{one section per detected endpoint, with inferred request/response schemas}
```

**Rules:**
- Infer request/response shapes from actual handler code — validation libs (Zod, Joi,
  Pydantic, class-validator) make this possible
- Mark endpoints that require auth vs public based on middleware detection
- Group by resource (auth, users, posts, etc.)
- Note rate limiting if detected in middleware

---

## Template 3 — Architecture Document

```markdown
# {project-name} — Architecture Overview

## System Overview

{3-4 sentence summary of what the system does and its scale targets}

## Architecture Diagram

```
{ASCII or Mermaid diagram — choose based on complexity}

Example Mermaid:
graph TB
    Client[Browser / Mobile]
    API[API Layer<br/>Next.js / Express]
    Auth[Auth Service<br/>JWT + Refresh Tokens]
    DB[(PostgreSQL<br/>via Prisma)]
    Cache[(Redis<br/>Sessions + Cache)]
    Storage[S3<br/>File Storage]

    Client --> API
    API --> Auth
    API --> DB
    API --> Cache
    API --> Storage
```

## Layers

### {Layer 1 — e.g. Presentation}
{files, responsibilities, key patterns}

### {Layer 2 — e.g. API / Route}
{files, responsibilities, key patterns}

### {Layer 3 — e.g. Service / Business Logic}
{files, responsibilities, key patterns}

### {Layer 4 — e.g. Data / Persistence}
{files, responsibilities, key patterns}

## Data Flow — {Core Feature}

{trace a key user-facing operation from HTTP request to DB response}

1. Request hits `{entry file}`
2. Passes through `{middleware}` for auth/validation
3. Routed to `{handler}`
4. Handler calls `{service}`
5. Service queries `{model/repo}` 
6. DB returns data, transformed in `{transformer/serialiser}`
7. Response sent as `{format}`

## Key Design Decisions

| Decision | Choice | Rationale |
|---|---|---|
{extract from code — ORM choice, auth approach, folder structure, etc.}

## Known Limitations & Tech Debt

{from issues_spotted in CKM + observations from code}
```

---

## Template 4 — Onboarding Guide

```markdown
# Onboarding Guide — {project-name}

> Get a new engineer from zero to productive in one session.

## What Is This Project?

{plain-English explanation — 1 paragraph}

## Before You Read Code

**Mental model**: {1-2 sentence conceptual model of the system}

**Key insight**: {the one thing that makes this codebase click once you understand it}

## Local Setup

{step-by-step from detected tooling — exact commands, no hand-waving}

```bash
# 1. Clone
git clone {url}
cd {repo}

# 2. Install dependencies
{exact install command}

# 3. Configure environment
cp .env.example .env
# Edit .env — required vars: {list critical ones}

# 4. Set up database
{migration command if detected}
{seed command if detected}

# 5. Start dev server
{start command}

# 6. Verify it's working
{what to check — URL, health endpoint, expected output}
```

## Files to Read First

Read these in order to build your mental model:

1. `{entry point}` — where everything starts
2. `{router/routes file}` — understand the surface area
3. `{main model/schema file}` — understand the data
4. `{core service file}` — understand the business logic
5. `{config file}` — understand the configuration

## Key Concepts in This Codebase

{explain 3-5 patterns or conventions specific to this project}

**{Convention 1}**: {explanation with example file}
**{Convention 2}**: {explanation with example file}
...

## How to Make Changes

### Adding a new API endpoint
{trace the exact files to touch, in order, based on the project's pattern}

### Adding a new data model
{trace the exact files — schema → migration → service → route}

### Running tests
```bash
{test commands}
```

## Gotchas

{list of non-obvious things a new engineer would trip on — inferred from the code}

- {gotcha 1}
- {gotcha 2}
...

## Who to Ask

{if CODEOWNERS or GitHub blame patterns suggest clear owners — note them}

---

*Generated by CAG from {repo} snapshot on {date}*
```

---

## Template 5 — Module Documentation

For a specific module or service file:

```markdown
# Module: {module_name}

**File**: `{filepath}`
**Category**: {Service / Model / Middleware / Utility / Handler}
**Dependencies**: {imports list}
**Used by**: {files that import this module}

## Purpose

{1-2 sentence plain-English description of what this module does}

## Exports

### `{functionName}({params})`

**Description**: {what it does}
**Parameters**:
- `{param}` ({type}): {description}

**Returns**: `{return type}` — {description}

**Example**:
```{language}
{usage example inferred from callers or tests}
```

**Notes**: {edge cases, side effects, things to be aware of}

---

{repeat for each export}

## Internal Logic

{brief walkthrough of non-obvious internal implementation details}

## Error Handling

{what errors this module throws/returns and when}

## Tests

**Test file**: `{test file path if detected}`
**Coverage**: {estimate — well-tested / partial / none}
```

---

## Output Rules (apply to all templates)

1. **Accuracy over completeness** — omit sections you can't fill accurately
2. **Show real paths** — always use actual file paths from the CKM, never `src/your-file.ts`
3. **Real commands only** — commands must match what's in package.json / Makefile / detected tooling
4. **Infer confidently** — you have the code; make definitive statements, not "probably" statements
5. **Format for GitHub** — use standard Markdown that renders well in GitHub README

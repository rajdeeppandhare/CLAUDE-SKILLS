---
name: docker-ship
description: >
  Docker Ship — GitHub repo to production-ready Docker setup. Use this skill whenever
  the user provides a GitHub repo URL and wants to containerise it, create a Dockerfile,
  set up docker-compose, or push to a container registry. Trigger on phrases like
  "dockerise this repo", "containerise my app", "create a Dockerfile for", "help me
  ship this to Docker", "set up docker-compose", "push to DockerHub", "push to GHCR",
  "create a Docker image", "how do I run this in Docker", or any GitHub link paired
  with Docker/container intent. Always use this skill for Docker + GitHub repo tasks.
---

# Docker Ship — GitHub Repo → Production Docker Setup

You are a senior DevOps engineer. Given a GitHub repository URL, you crawl the codebase,
detect the full stack, and produce a complete, production-ready Docker setup — not tutorial
boilerplate, but real files ready to build and ship.

Your output posture: **opinionated, production-grade, immediately usable**. Every file
you produce should be copy-paste ready. You never produce `node:latest` or toy single-stage
Dockerfiles. You build what a senior engineer would build.

---

## The Docker Ship Pipeline

```
PHASE 1 — CRAWL        Fetch repo structure + key files via GitHub API
PHASE 2 — DETECT       Identify stack, runtime, DB, ports, env vars
PHASE 3 — AUDIT        Check for existing Docker files — improve or replace
PHASE 4 — GENERATE     Produce the full Docker setup package
PHASE 5 — GUIDE        Exact commands to build, run, push, deploy
```

---

## Phase 1 — Crawl

Fetch the repo file tree:
```
GET https://api.github.com/repos/{owner}/{repo}/git/trees/HEAD?recursive=1
```

Also fetch repo metadata:
```
GET https://api.github.com/repos/{owner}/{repo}
```

If rate-limited or private, fall back to fetching files directly:
```
https://raw.githubusercontent.com/{owner}/{repo}/{branch}/{filepath}
```

**Branch detection**: try `main` → `master` → `develop` → check `default_branch` in metadata.

If the repo is private, ask for a GitHub personal access token:
```
Authorization: token ghp_xxx
```

---

## Phase 2 — Detect

### Stack Detection Priority Files

Fetch in this order, stopping once stack is confirmed:

**Runtime Detection**
```
package.json          → Node.js (extract: node version from engines.node or .nvmrc)
.nvmrc / .node-version → Node.js version pin
requirements.txt      → Python
pyproject.toml        → Python (check [tool.poetry] or [build-system])
setup.py / Pipfile    → Python
.python-version       → Python version pin
go.mod                → Go (extract: go version)
Cargo.toml            → Rust (extract: edition)
pom.xml               → Java/Maven (extract: java.version)
build.gradle          → Java/Gradle
gradlew               → Java (confirms Gradle wrapper)
Gemfile               → Ruby (extract: ruby version)
composer.json         → PHP (extract: php version)
*.csproj / *.sln      → .NET
```

**App Type Detection**
```
# Frontend indicators
next.config.* / nuxt.config.* / vite.config.*  → SSR/SSG framework
src/pages/ or app/  + package.json              → Next.js/similar
public/ + index.html                            → Static site

# Backend indicators
server.js / app.js / main.py / main.go / cmd/  → Server entry point
routes/ / controllers/ / handlers/             → API server
manage.py                                      → Django
wsgi.py / asgi.py                              → Python WSGI/ASGI app

# Full-stack indicators
Both frontend + backend dirs → multi-stage build needed
```

**Database Detection**
```
prisma/schema.prisma                → PostgreSQL (usually)
drizzle.config.*                    → check dialect field
knexfile.*                          → check client field
models/ with sequelize              → check dialect
database.yml (Rails)                → adapter field
alembic.ini / migrations/           → Python + SQL DB
docker-compose.yml (existing)       → read services
.env.example                        → scan for DB_URL, DATABASE_URL, POSTGRES_*, MYSQL_*, MONGO_*, REDIS_*
Any source file imports             → pg, mysql2, mongodb, redis, sqlite3
```

**Port Detection**
```
.env.example                        → PORT=
server config files                 → app.listen(PORT), port: 8080
package.json scripts                → --port flags
framework defaults:
  Express/Fastify   → 3000
  Next.js           → 3000
  Django            → 8000
  Flask             → 5000
  FastAPI           → 8000
  Go net/http       → 8080
  Rails             → 3000
  Spring Boot       → 8080
  .NET              → 5000/5001
```

**Env Var Extraction**
Scan these files for all env var references:
```
.env.example / .env.sample / .env.template
Any source file: process.env.*, os.environ.*, os.getenv(), viper.Get()
config/ files
```

Build a complete list: `VAR_NAME=description_or_example_value`

### Detection Output — Build the Stack Profile

```
STACK_PROFILE = {
  runtime: "Node.js 20",
  framework: "Next.js 14",
  package_manager: "npm / yarn / pnpm / bun",
  build_command: "npm run build",
  start_command: "npm start",
  port: 3000,
  databases: ["PostgreSQL 15", "Redis 7"],
  has_migrations: true,
  migration_command: "npx prisma migrate deploy",
  is_fullstack: false,
  has_frontend_build: true,
  static_output_dir: ".next",
  env_vars: [
    "DATABASE_URL=postgresql://user:pass@localhost:5432/db",
    "REDIS_URL=redis://localhost:6379",
    "JWT_SECRET=",
    "PORT=3000"
  ],
  existing_dockerfile: false,
  existing_compose: false
}
```

---

## Phase 3 — Audit Existing Docker Files

If a `Dockerfile` already exists, fetch and audit it:

**Check for these issues:**
- Using `latest` tag → replace with pinned version
- No multi-stage build when there's a build step → add stages
- Running as root → add non-root user
- No `.dockerignore` → create one
- COPY . . before installing deps → reorder for layer caching
- No health check → add HEALTHCHECK
- Secrets hardcoded → flag and remove
- `npm install` instead of `npm ci` → fix
- No `NODE_ENV=production` → add

If the existing Dockerfile is fixable, improve it with comments explaining each change.
If it's fundamentally wrong, replace it entirely and explain why.

---

## Phase 4 — Generate the Docker Setup Package

Produce ALL of these files. Every file must be complete and ready to use.

### 4.1 — Dockerfile

**Node.js (with build step — Next.js example)**
```dockerfile
# syntax=docker/dockerfile:1
FROM node:20-alpine AS base
WORKDIR /app
ENV NODE_ENV=production

# Install dependencies (cached layer)
FROM base AS deps
COPY package.json package-lock.json ./
RUN npm ci --only=production

# Build stage
FROM base AS builder
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production image
FROM base AS runner
RUN addgroup --system --gid 1001 nodejs \
 && adduser --system --uid 1001 nextjs

COPY --from=deps /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json

USER nextjs
EXPOSE 3000
ENV PORT=3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -qO- http://localhost:3000/api/health || exit 1

CMD ["npm", "start"]
```

**Python (FastAPI/Flask/Django)**
```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1
WORKDIR /app

FROM base AS deps
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM base AS runner
RUN addgroup --system --gid 1001 appgroup \
 && adduser --system --uid 1001 --ingroup appgroup appuser

COPY --from=deps /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=deps /usr/local/bin /usr/local/bin
COPY . .

USER appuser
EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Go**
```dockerfile
# syntax=docker/dockerfile:1
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o server ./cmd/server

FROM scratch AS runner
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /app/server /server
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s CMD ["/server", "-health"]
ENTRYPOINT ["/server"]
```

**Ruby on Rails**
```dockerfile
# syntax=docker/dockerfile:1
FROM ruby:3.3-slim AS base
ENV BUNDLE_WITHOUT="development test" \
    RAILS_ENV=production \
    RAILS_SERVE_STATIC_FILES=true
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential libpq-dev nodejs yarn \
 && rm -rf /var/lib/apt/lists/*

FROM base AS deps
COPY Gemfile Gemfile.lock ./
RUN bundle install --jobs 4 --retry 3

FROM base AS runner
RUN addgroup --system --gid 1001 rails \
 && adduser --system --uid 1001 --ingroup rails railsuser
COPY --from=deps /usr/local/bundle /usr/local/bundle
COPY . .
RUN bundle exec rake assets:precompile
USER railsuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=5s CMD curl -f http://localhost:3000/up || exit 1
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]
```

**Java (Maven/Spring Boot)**
```dockerfile
# syntax=docker/dockerfile:1
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /app
COPY pom.xml .
COPY mvnw ./
COPY .mvn .mvn
RUN ./mvnw dependency:resolve
COPY src src
RUN ./mvnw package -DskipTests

FROM eclipse-temurin:21-jre-alpine AS runner
RUN addgroup --system --gid 1001 spring \
 && adduser --system --uid 1001 --ingroup spring springuser
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
USER springuser
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=5s CMD wget -qO- http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Always adapt to the actual detected stack. Pinned versions must match what's detected
in the repo (e.g. if `.nvmrc` says `20.11.0`, use `node:20.11.0-alpine` not just `node:20-alpine`).

### 4.2 — .dockerignore

Generate language-aware, always include:
```
# Git
.git
.gitignore
.gitattributes

# Docker
Dockerfile*
docker-compose*
.dockerignore

# Docs
README*
CHANGELOG*
*.md
docs/

# Tests
__tests__/
tests/
test/
spec/
coverage/
.nyc_output/

# Dev tooling
.eslintrc*
.prettierrc*
.editorconfig
.husky/
.github/

# Environment
.env
.env.*
!.env.example

# OS
.DS_Store
Thumbs.db

# [Language-specific additions below]
```

Node.js additions:
```
node_modules/
npm-debug.log*
.npm/
*.tgz
.next/cache/
```

Python additions:
```
__pycache__/
*.pyc
*.pyo
.venv/
venv/
*.egg-info/
dist/
build/
.pytest_cache/
```

Go additions:
```
vendor/
*.exe
*.test
```

### 4.3 — docker-compose.yml

Always include the app service plus any detected databases:

```yaml
# docker-compose.yml — {repo-name}
# Generated by Docker Ship skill

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      target: runner
    image: {repo-name}:latest
    container_name: {repo-name}-app
    restart: unless-stopped
    ports:
      - "${PORT:-3000}:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=${DATABASE_URL}
      # Add all detected env vars here
    env_file:
      - .env
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost:3000/api/health"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s
    networks:
      - app-network

  # Add only detected databases:

  db:   # Only if PostgreSQL detected
    image: postgres:15-alpine
    container_name: {repo-name}-db
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-appdb}
      POSTGRES_USER: ${POSTGRES_USER:-appuser}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:?POSTGRES_PASSWORD required}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER:-appuser}"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - app-network

  redis:  # Only if Redis detected
    image: redis:7-alpine
    container_name: {repo-name}-redis
    restart: unless-stopped
    command: redis-server --appendonly yes
    volumes:
      - redis-data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 3
    networks:
      - app-network

volumes:
  postgres-data:
  redis-data:

networks:
  app-network:
    driver: bridge
```

If migrations exist, add an `migrate` service that runs before `app`:
```yaml
  migrate:
    build:
      context: .
      dockerfile: Dockerfile
    command: npx prisma migrate deploy   # or detected migration command
    environment:
      - DATABASE_URL=${DATABASE_URL}
    depends_on:
      db:
        condition: service_healthy
    networks:
      - app-network
```

### 4.4 — .env.example

Extract ALL detected env vars, with sensible example values:
```bash
# Application
NODE_ENV=production
PORT=3000

# Database
DATABASE_URL=postgresql://appuser:changeme@db:5432/appdb
POSTGRES_DB=appdb
POSTGRES_USER=appuser
POSTGRES_PASSWORD=changeme

# Cache
REDIS_URL=redis://redis:6379

# Auth (change these!)
JWT_SECRET=change-this-to-a-long-random-string
SESSION_SECRET=change-this-too

# Any other vars detected in source code
```

### 4.5 — Makefile

```makefile
# Makefile — {repo-name} Docker shortcuts
REPO_NAME := {repo-name}
IMAGE_NAME := {dockerhub-user}/{repo-name}
VERSION    := $(shell git describe --tags --always --dirty 2>/dev/null || echo "dev")

.PHONY: build run stop logs shell push clean

build:          ## Build the Docker image
	docker build -t $(IMAGE_NAME):$(VERSION) -t $(IMAGE_NAME):latest .

run:            ## Start all services
	docker compose up -d

stop:           ## Stop all services
	docker compose down

logs:           ## Tail app logs
	docker compose logs -f app

shell:          ## Open shell in running app container
	docker compose exec app sh

push:           ## Push image to DockerHub
	docker push $(IMAGE_NAME):$(VERSION)
	docker push $(IMAGE_NAME):latest

push-ghcr:      ## Push image to GitHub Container Registry
	docker tag $(IMAGE_NAME):latest ghcr.io/{owner}/$(REPO_NAME):latest
	docker push ghcr.io/{owner}/$(REPO_NAME):latest

clean:          ## Remove containers and volumes
	docker compose down -v --remove-orphans

migrate:        ## Run database migrations
	docker compose run --rm migrate

.DEFAULT_GOAL := help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-15s\033[0m %s\n", $$1, $$2}'
```

### 4.6 — README-DOCKER.md

```markdown
# Docker Setup — {repo-name}

## Prerequisites
- Docker 24+ and Docker Compose v2+
- (Optional) DockerHub or GHCR account for pushing

## Quick Start

\`\`\`bash
# 1. Clone the repo
git clone https://github.com/{owner}/{repo-name}
cd {repo-name}

# 2. Set up environment
cp .env.example .env
# Edit .env — at minimum set passwords and secrets

# 3. Build and start
make build
make run

# 4. Run migrations (if needed)
make migrate

# App is now running at http://localhost:{PORT}
\`\`\`

## Building the Image

\`\`\`bash
docker build -t {repo-name}:latest .
\`\`\`

## Running with Docker Compose

\`\`\`bash
docker compose up -d        # Start all services
docker compose logs -f app  # Watch logs
docker compose down         # Stop everything
\`\`\`

## Pushing to DockerHub

\`\`\`bash
docker login
docker tag {repo-name}:latest yourusername/{repo-name}:latest
docker push yourusername/{repo-name}:latest
\`\`\`

## Pushing to GitHub Container Registry (GHCR)

\`\`\`bash
echo $GITHUB_TOKEN | docker login ghcr.io -u {owner} --password-stdin
docker tag {repo-name}:latest ghcr.io/{owner}/{repo-name}:latest
docker push ghcr.io/{owner}/{repo-name}:latest
\`\`\`

## Environment Variables

| Variable | Required | Description |
|---|---|---|
{env var table generated from .env.example}

## Services

| Service | Port | Description |
|---|---|---|
| app | {PORT} | Main application |
| db | 5432 | PostgreSQL database |
| redis | 6379 | Redis cache |

## Image Details

- Base: `{detected-base-image}`
- Multi-stage: Yes
- Runs as root: No (user: `{appuser}`)
- Health check: Yes (`/api/health` or detected endpoint)
- Approximate image size: ~{estimated}MB
```

---

## Phase 5 — Output Report

After generating all files, produce this report:

```
╔══════════════════════════════════════════════════════════╗
║            DOCKER SHIP — SETUP COMPLETE                  ║
╚══════════════════════════════════════════════════════════╝

📦 REPO:      {owner}/{repo}
🔧 STACK:     {runtime} + {framework}
🗄️  DATABASE:  {detected databases or "none"}
🌐 PORT:      {port}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 FILES GENERATED
   ✅ Dockerfile           — {stages}-stage build, ~{size}MB estimated
   ✅ .dockerignore        — {n} exclusions
   ✅ docker-compose.yml   — {n} services
   ✅ .env.example         — {n} env vars
   ✅ Makefile             — build/run/push shortcuts
   ✅ README-DOCKER.md     — full usage guide

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚡ QUICK START
   cp .env.example .env   # then edit passwords/secrets
   make build             # build the image
   make run               # start everything
   make migrate           # run DB migrations (if needed)
   # → App at http://localhost:{port}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️  THINGS TO DO BEFORE DEPLOYING
   → Change all passwords in .env (especially POSTGRES_PASSWORD)
   → Set a strong JWT_SECRET / SESSION_SECRET
   → {any other repo-specific warnings}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 DESIGN DECISIONS
   → Multi-stage build: keeps final image small (no dev deps)
   → Non-root user: security best practice
   → Health checks: compose waits for DB before starting app
   → Layer caching: deps copied before source for fast rebuilds
   → {any repo-specific decisions}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 PUSH TO REGISTRY
   DockerHub:  make push
   GHCR:       make push-ghcr
```

---

## Guardrails

- **Never hardcode secrets** — always reference env vars; flag any found in repo
- **Always pin versions** — no `latest` tags in generated Dockerfiles
- **Match detected versions** — if `.nvmrc` says `20.11.0`, use that exact version
- **Non-root always** — every generated Dockerfile must run as a non-root user
- **Health checks always** — every service in compose must have a healthcheck
- **Detect the actual port** — don't assume; read the code/config
- **Existing Dockerfile** — audit before replacing; explain every change
- **Private repos** — 404/403 → ask for PAT immediately
- **Monorepos** — map each service separately, produce one compose with all services

---

## Reference Files

| File | Load When |
|---|---|
| `references/dockerfile-patterns.md` | Generating Dockerfiles for any stack |
| `references/compose-patterns.md` | Generating docker-compose or troubleshooting services |

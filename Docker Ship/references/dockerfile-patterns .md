# Dockerfile Patterns Reference

## Core Principles (Non-Negotiable)

1. **Pin base image versions** — `node:20.11.0-alpine` not `node:latest`
2. **Multi-stage builds** — always when there's a build step
3. **Non-root user** — always; add group + user, COPY files, then USER
4. **Layer cache optimisation** — COPY deps first, install, then COPY source
5. **Health checks** — every production Dockerfile needs HEALTHCHECK
6. **Minimal base** — alpine or slim variants; scratch for Go/Rust binaries

---

## Base Image Selection Guide

| Runtime | Production Base | Notes |
|---|---|---|
| Node.js | `node:{version}-alpine` | Use exact version from .nvmrc |
| Python | `python:{version}-slim` | slim over alpine (C extension compat) |
| Go | `scratch` or `gcr.io/distroless/static` | Copy binary only |
| Rust | `scratch` or `debian:bookworm-slim` | Static binary preferred |
| Java | `eclipse-temurin:{version}-jre-alpine` | JRE not JDK for runtime |
| Ruby | `ruby:{version}-slim` | slim over alpine for native gems |
| PHP | `php:{version}-fpm-alpine` | + nginx in compose |
| .NET | `mcr.microsoft.com/dotnet/aspnet:{version}` | Official Microsoft image |

---

## Multi-Stage Pattern Templates

### Node.js — API Server (no frontend build)
```dockerfile
FROM node:20-alpine AS base
WORKDIR /app
ENV NODE_ENV=production

FROM base AS deps
COPY package.json package-lock.json ./
RUN npm ci --only=production

FROM base AS runner
RUN addgroup -S nodejs && adduser -S nodeapp -G nodejs
COPY --from=deps /app/node_modules ./node_modules
COPY --chown=nodeapp:nodejs . .
USER nodeapp
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:3000/health || exit 1
CMD ["node", "src/index.js"]
```

### Node.js — Full-stack (Next.js / Nuxt / SvelteKit)
```dockerfile
FROM node:20-alpine AS base
WORKDIR /app

FROM base AS deps
COPY package.json package-lock.json ./
RUN npm ci

FROM base AS builder
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM base AS runner
ENV NODE_ENV=production
RUN addgroup -S nodejs && adduser -S nextjs -G nodejs
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static
COPY --from=builder --chown=nextjs:nodejs /app/public ./public
USER nextjs
EXPOSE 3000
ENV PORT=3000 HOSTNAME=0.0.0.0
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:3000/api/health || exit 1
CMD ["node", "server.js"]
```
Note: Next.js standalone output requires `output: 'standalone'` in next.config.js. Add this note in README-DOCKER.md if not already set.

### Python — FastAPI / Uvicorn
```dockerfile
FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
WORKDIR /app

FROM base AS deps
RUN pip install --upgrade pip
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM base AS runner
RUN groupadd --system appgroup && useradd --system --gid appgroup appuser
COPY --from=deps /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=deps /usr/local/bin/uvicorn /usr/local/bin/uvicorn
COPY --chown=appuser:appgroup . .
USER appuser
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=5s CMD curl -f http://localhost:8000/health || exit 1
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--workers", "2"]
```

### Python — Django / Gunicorn
```dockerfile
FROM python:3.11-slim AS base
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1 DJANGO_SETTINGS_MODULE=config.settings.production
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends libpq-dev && rm -rf /var/lib/apt/lists/*

FROM base AS deps
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM base AS runner
RUN groupadd --system django && useradd --system --gid django djangouser
COPY --from=deps /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=deps /usr/local/bin /usr/local/bin
COPY --chown=djangouser:django . .
RUN python manage.py collectstatic --noinput
USER djangouser
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=5s CMD curl -f http://localhost:8000/health/ || exit 1
CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8000", "--workers", "3"]
```

### Go — Minimal Binary
```dockerfile
FROM golang:1.22-alpine AS builder
WORKDIR /app
RUN apk add --no-cache git ca-certificates
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build \
    -ldflags="-w -s -X main.version=$(git describe --tags 2>/dev/null || echo dev)" \
    -o server ./cmd/server

FROM scratch
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /app/server /server
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s CMD ["/server", "-healthcheck"]
ENTRYPOINT ["/server"]
```
Note: `scratch` has no shell. If debugging is needed, use `gcr.io/distroless/static-debian12` instead.

### Rust — Minimal Binary
```dockerfile
FROM rust:1.76-slim AS builder
WORKDIR /app
COPY Cargo.toml Cargo.lock ./
RUN mkdir src && echo "fn main(){}" > src/main.rs && cargo build --release && rm -rf src
COPY src ./src
RUN touch src/main.rs && cargo build --release

FROM debian:bookworm-slim AS runner
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates && rm -rf /var/lib/apt/lists/*
RUN groupadd --system appgroup && useradd --system --gid appgroup appuser
COPY --from=builder /app/target/release/server /usr/local/bin/server
USER appuser
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost:8080/health || exit 1
CMD ["/usr/local/bin/server"]
```
Note: The dummy src/main.rs trick caches dependency compilation separately from source changes.

### Java — Spring Boot (Maven)
```dockerfile
FROM eclipse-temurin:21-jdk-alpine AS builder
WORKDIR /build
COPY pom.xml mvnw ./
COPY .mvn .mvn
RUN ./mvnw dependency:go-offline -q
COPY src ./src
RUN ./mvnw package -DskipTests -q

FROM eclipse-temurin:21-jre-alpine AS runner
RUN addgroup --system spring && adduser --system --ingroup spring springuser
WORKDIR /app
ARG JAR_FILE=/build/target/*.jar
COPY --from=builder ${JAR_FILE} app.jar
USER springuser
EXPOSE 8080
HEALTHCHECK --interval=30s --timeout=5s --start-period=30s \
  CMD wget -qO- http://localhost:8080/actuator/health || exit 1
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## Common Issues and Fixes

### Issue: `npm install` should be `npm ci`
```dockerfile
# Wrong
RUN npm install

# Correct — uses lockfile, reproducible builds
RUN npm ci --only=production
```

### Issue: No layer caching for deps
```dockerfile
# Wrong — invalidates cache on any source change
COPY . .
RUN npm install

# Correct — only reinstalls when package.json changes
COPY package.json package-lock.json ./
RUN npm ci --only=production
COPY . .
```

### Issue: Running as root
```dockerfile
# Wrong
# (no USER directive = runs as root)

# Correct
RUN addgroup --system --gid 1001 appgroup \
 && adduser --system --uid 1001 --ingroup appgroup appuser
USER appuser
```

### Issue: Secrets in ENV
```dockerfile
# Wrong — baked into image layer history
ENV API_KEY=sk-real-secret-here

# Correct — passed at runtime
ENV API_KEY=""
# Document in .env.example and README
```

### Issue: Missing HEALTHCHECK
```dockerfile
# Always add — docker compose depends_on condition: service_healthy requires it
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -qO- http://localhost:${PORT}/health || exit 1
```

---

## Estimated Image Sizes by Stack

| Stack | Naive | Optimised |
|---|---|---|
| Node.js API | ~900MB | ~150MB (alpine + no devDeps) |
| Next.js | ~1.2GB | ~200MB (standalone output) |
| Python FastAPI | ~800MB | ~180MB (slim + no cache) |
| Django | ~900MB | ~200MB |
| Go binary | ~800MB | ~8MB (scratch) |
| Rust binary | ~1.5GB | ~15MB (scratch/distroless) |
| Java Spring | ~600MB | ~180MB (JRE only) |
| Ruby on Rails | ~1.1GB | ~350MB (slim) |

---

## pyproject.toml vs requirements.txt

If repo uses `pyproject.toml` with Poetry:
```dockerfile
FROM python:3.11-slim AS base
ENV POETRY_NO_INTERACTION=1 POETRY_VENV_IN_PROJECT=1
RUN pip install poetry==1.8.2

FROM base AS deps
COPY pyproject.toml poetry.lock ./
RUN poetry install --only=main --no-root

FROM base AS runner
COPY --from=deps /app/.venv /app/.venv
ENV PATH="/app/.venv/bin:$PATH"
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

If repo uses `pyproject.toml` with pip (PEP 517):
```dockerfile
COPY pyproject.toml .
RUN pip install --no-cache-dir .
```

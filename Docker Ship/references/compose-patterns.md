# Docker Compose Patterns Reference

## Core Principles (Non-Negotiable)

1. **Pin image versions** — `postgres:16-alpine` not `postgres:latest`
2. **Health checks on every service** — `depends_on: condition: service_healthy` requires it
3. **Named volumes** — never anonymous volumes for persistent data
4. **Custom networks** — always; never rely on default bridge
5. **Restart policy** — `unless-stopped` for production, `on-failure` for jobs
6. **Secrets via env vars** — always `${VAR}` with `.env`; never hardcoded

---

## Full Production Compose Template

```yaml
# docker-compose.yml
# Usage: cp .env.example .env && docker compose up -d

services:

  app:
    build:
      context: .
      dockerfile: Dockerfile
      target: runner          # target the production stage
    container_name: {repo}-app
    restart: unless-stopped
    ports:
      - "${PORT:-3000}:${PORT:-3000}"
    environment:
      NODE_ENV: production
      PORT: ${PORT:-3000}
      DATABASE_URL: ${DATABASE_URL}
      REDIS_URL: ${REDIS_URL}
    env_file:
      - .env
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "wget -qO- http://localhost:${PORT:-3000}/health || exit 1"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s

  db:
    image: postgres:16-alpine
    container_name: {repo}-db
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-appdb}
      POSTGRES_USER: ${POSTGRES_USER:-appuser}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:?POSTGRES_PASSWORD is required}
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./scripts/init.sql:/docker-entrypoint-initdb.d/init.sql:ro  # optional seed
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER:-appuser} -d ${POSTGRES_DB:-appdb}"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s

  redis:
    image: redis:7-alpine
    container_name: {repo}-redis
    restart: unless-stopped
    command: redis-server --appendonly yes --requirepass ${REDIS_PASSWORD:-}
    volumes:
      - redis-data:/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 3

volumes:
  postgres-data:
    driver: local
  redis-data:
    driver: local

networks:
  app-network:
    driver: bridge
```

---

## Service Templates by Database

### PostgreSQL
```yaml
  db:
    image: postgres:16-alpine
    container_name: {repo}-db
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-appdb}
      POSTGRES_USER: ${POSTGRES_USER:-appuser}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:?required}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER:-appuser}"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s
```

### MySQL / MariaDB
```yaml
  db:
    image: mysql:8.0
    container_name: {repo}-db
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE:-appdb}
      MYSQL_USER: ${MYSQL_USER:-appuser}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD:?required}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:?required}
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "${MYSQL_USER:-appuser}", "-p${MYSQL_PASSWORD}"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 15s
```

### MongoDB
```yaml
  mongo:
    image: mongo:7.0
    container_name: {repo}-mongo
    restart: unless-stopped
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGO_USER:-admin}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_PASSWORD:?required}
      MONGO_INITDB_DATABASE: ${MONGO_DB:-appdb}
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network
    healthcheck:
      test: echo 'db.runCommand("ping").ok' | mongosh localhost:27017/test --quiet
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 20s
```

### Redis
```yaml
  redis:
    image: redis:7-alpine
    container_name: {repo}-redis
    restart: unless-stopped
    command: redis-server --appendonly yes
    volumes:
      - redis-data:/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 3
```

### Elasticsearch
```yaml
  elasticsearch:
    image: elasticsearch:8.12.0
    container_name: {repo}-es
    restart: unless-stopped
    environment:
      discovery.type: single-node
      ES_JAVA_OPTS: -Xms512m -Xmx512m
      xpack.security.enabled: "false"
    volumes:
      - es-data:/usr/share/elasticsearch/data
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "curl -s http://localhost:9200/_cluster/health | grep -q '\"status\":\"green\\|yellow\"'"]
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 30s
```

---

## Migration Service Pattern

Always run migrations before the app starts. Use a separate service that exits 0 on success:

```yaml
  migrate:
    build:
      context: .
      dockerfile: Dockerfile
      target: runner
    container_name: {repo}-migrate
    command: {migration_command}    # e.g. npx prisma migrate deploy
    environment:
      DATABASE_URL: ${DATABASE_URL}
    depends_on:
      db:
        condition: service_healthy
    networks:
      - app-network
    restart: "no"    # run once and exit
```

Then in the `app` service:
```yaml
    depends_on:
      migrate:
        condition: service_completed_successfully
      db:
        condition: service_healthy
```

### Migration commands by stack
```
Prisma:         npx prisma migrate deploy
Django:         python manage.py migrate
Rails:          bundle exec rails db:migrate
Alembic:        alembic upgrade head
Flyway:         flyway migrate
Liquibase:      liquibase update
golang-migrate: migrate -path ./migrations -database ${DATABASE_URL} up
```

---

## Dev Override Pattern (docker-compose.override.yml)

For local development, use a compose override — keeps production compose clean:

```yaml
# docker-compose.override.yml  (auto-loaded by `docker compose up`)
services:
  app:
    build:
      target: base          # use dev stage, not production runner
    volumes:
      - .:/app              # live reload — mount source
      - /app/node_modules   # anonymous volume to protect node_modules
    environment:
      NODE_ENV: development
    command: npm run dev    # override production start command
    ports:
      - "9229:9229"         # Node.js debug port

  db:
    ports:
      - "5432:5432"         # expose DB port locally for DB client access

  redis:
    ports:
      - "6379:6379"         # expose Redis locally
```

Run production: `docker compose -f docker-compose.yml up`
Run dev: `docker compose up` (auto-merges override)

---

## Profiles Pattern

Use profiles to make optional services opt-in:

```yaml
services:
  app:
    # no profile — always runs

  pgadmin:
    image: dpage/pgadmin4:latest
    profiles: ["tools"]         # only starts with: docker compose --profile tools up
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_EMAIL:-admin@admin.com}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_PASSWORD:-admin}
    ports:
      - "5050:80"
    networks:
      - app-network

  mailhog:
    image: mailhog/mailhog
    profiles: ["tools"]
    ports:
      - "1025:1025"   # SMTP
      - "8025:8025"   # Web UI
    networks:
      - app-network
```

---

## Nginx Reverse Proxy Pattern

When the app doesn't serve HTTPS directly, add nginx:

```yaml
  nginx:
    image: nginx:1.25-alpine
    container_name: {repo}-nginx
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro    # mount certs
      - static-files:/var/www/static:ro  # if serving static assets
    depends_on:
      app:
        condition: service_healthy
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "nginx", "-t"]
      interval: 30s
      timeout: 5s
      retries: 3
```

Minimal `nginx/nginx.conf`:
```nginx
events {}
http {
    upstream app {
        server app:{PORT};
    }
    server {
        listen 80;
        location / {
            proxy_pass http://app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

---

## Common Issues and Fixes

### Issue: App starts before DB is ready
```yaml
# Wrong
depends_on:
  - db

# Correct — waits for health check to pass
depends_on:
  db:
    condition: service_healthy
```
Requires a `healthcheck` on the `db` service.

### Issue: Data lost on container restart
```yaml
# Wrong — anonymous volume
volumes:
  - /var/lib/postgresql/data

# Correct — named volume (persists across down/up)
volumes:
  - postgres-data:/var/lib/postgresql/data
# + declare at top level:
volumes:
  postgres-data:
```

### Issue: Secrets in docker-compose.yml
```yaml
# Wrong
environment:
  DATABASE_URL: postgresql://user:hardcoded@db:5432/appdb

# Correct — reference env vars
environment:
  DATABASE_URL: ${DATABASE_URL}
# Or use env_file:
env_file:
  - .env
```

### Issue: Ports exposed unnecessarily in production
```yaml
# For production — don't expose DB ports to host
# db service: no ports: entry (only accessible within app-network)

# For local dev only — use override:
# docker-compose.override.yml:
services:
  db:
    ports:
      - "5432:5432"
```

### Issue: No restart policy
```yaml
# Always set for production services
restart: unless-stopped

# One-shot jobs (migrations, seeds):
restart: "no"
```

---

## Healthcheck Recipes by Service

```yaml
# PostgreSQL
test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]

# MySQL
test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]

# MongoDB
test: echo 'db.runCommand("ping").ok' | mongosh localhost:27017/test --quiet

# Redis
test: ["CMD", "redis-cli", "ping"]

# Node.js HTTP
test: ["CMD-SHELL", "wget -qO- http://localhost:3000/health || exit 1"]

# Python HTTP
test: ["CMD-SHELL", "curl -f http://localhost:8000/health || exit 1"]

# Generic TCP port check
test: ["CMD-SHELL", "nc -z localhost 3000 || exit 1"]

# Elasticsearch
test: ["CMD-SHELL", "curl -s http://localhost:9200/_cluster/health | grep -q 'green\\|yellow'"]
```

---

## Estimated Resource Usage (for sizing)

| Service | Memory | CPU |
|---|---|---|
| Node.js app | 128–512MB | 0.25–1.0 |
| Python app | 128–256MB | 0.25–0.5 |
| Go app | 32–128MB | 0.1–0.5 |
| PostgreSQL | 256MB–2GB | 0.5–2.0 |
| MySQL | 256MB–1GB | 0.5–1.5 |
| Redis | 32–256MB | 0.1–0.5 |
| MongoDB | 256MB–2GB | 0.5–2.0 |
| Elasticsearch | 1–4GB | 1.0–4.0 |
| Nginx | 32–64MB | 0.1 |

Add resource limits to compose for production:
```yaml
    deploy:
      resources:
        limits:
          memory: 512M
          cpus: '0.5'
        reservations:
          memory: 128M
```

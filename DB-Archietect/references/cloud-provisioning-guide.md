# Cloud Provisioning Guide

Step-by-step instructions to provision each database on each major cloud/platform. Load this when generating SETUP.md files or when the user asks how to actually spin up a database instance.

---

## PostgreSQL via Supabase (recommended for most projects)

### Provision
1. Go to [supabase.com](https://supabase.com) → New Project
2. Choose organisation, set project name, set database password (save it — used in connection string)
3. Select region closest to your users
4. Wait ~2 minutes for provisioning

### Get Connection Strings
Dashboard → Project Settings → Database → Connection string

```
# Standard (port 5432) — for local dev, migrations
postgresql://postgres:[YOUR-PASSWORD]@db.[PROJECT-REF].supabase.co:5432/postgres

# Pooled / Transaction (port 6543) — for serverless / Vercel production
postgresql://postgres.[PROJECT-REF]:[YOUR-PASSWORD]@aws-0-[REGION].pooler.supabase.com:6543/postgres

# Session pooler (port 5432 on pooler host) — for persistent server apps
postgresql://postgres.[PROJECT-REF]:[YOUR-PASSWORD]@aws-0-[REGION].pooler.supabase.com:5432/postgres
```

### Apply Schema
```bash
# Option A: Supabase SQL Editor (Dashboard → SQL Editor → New query → paste schema.sql)

# Option B: psql
psql "postgresql://postgres:[PASSWORD]@db.[REF].supabase.co:5432/postgres" -f schema.sql

# Option C: Prisma migrations
DATABASE_URL="..." DIRECT_URL="..." npx prisma migrate deploy
```

### Enable Row-Level Security (if multi-tenant)
```sql
-- In SQL Editor, for each tenant-scoped table:
ALTER TABLE your_table ENABLE ROW LEVEL SECURITY;
```

### Environment Variables
```env
DATABASE_URL="postgresql://postgres.[REF]:[PASSWORD]@aws-0-[REGION].pooler.supabase.com:6543/postgres?pgbouncer=true"
DIRECT_URL="postgresql://postgres:[PASSWORD]@db.[REF].supabase.co:5432/postgres"
NEXT_PUBLIC_SUPABASE_URL="https://[REF].supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="[anon key from Dashboard → Settings → API]"
SUPABASE_SERVICE_ROLE_KEY="[service role key — server-side only, never expose]"
```

---

## PostgreSQL via Neon (best for serverless / Next.js)

### Provision
1. Go to [neon.tech](https://neon.tech) → Sign up → New Project
2. Set project name and region
3. Copy the connection string from the dashboard

### Get Connection String
```
postgresql://[USER]:[PASSWORD]@[HOST].neon.tech/[DBNAME]?sslmode=require
```

### Branching (killer feature)
```bash
# Create a branch for each PR / preview deployment
# Dashboard → Branches → New Branch
# Or via Neon CLI:
npm install -g neonctl
neonctl auth
neonctl branches create --name=preview/my-feature --project-id=[PROJECT_ID]
```

### Apply Schema
```bash
npx prisma migrate deploy
# or
psql "[CONNECTION_STRING]" -f schema.sql
```

### Environment Variables
```env
DATABASE_URL="postgresql://[USER]:[PASSWORD]@[HOST].neon.tech/[DB]?sslmode=require"
```

### Neon Serverless Driver (edge-compatible)
```bash
npm install @neondatabase/serverless
```
```typescript
import { neon } from '@neondatabase/serverless'
const sql = neon(process.env.DATABASE_URL!)
const result = await sql`SELECT * FROM users WHERE id = ${userId}`
```

---

## MongoDB Atlas

### Provision
1. Go to [cloud.mongodb.com](https://cloud.mongodb.com) → Create → Free (M0) or Dedicated
2. Choose cloud provider (AWS/GCP/Azure) and region
3. Set username and password (Database Access → Add new user)
4. Whitelist IP (Network Access → Add IP Address → `0.0.0.0/0` for dev, specific IPs for prod)

### Get Connection String
Clusters → Connect → Drivers → Node.js
```
mongodb+srv://[USERNAME]:[PASSWORD]@[CLUSTER].mongodb.net/[DATABASE]?retryWrites=true&w=majority
```

### Create Collections + Indexes
```javascript
// Via Mongoose (schemas auto-create collections on first write)
// Or via Atlas UI: Collections → Create Collection

// Apply indexes via code (call on app startup):
await User.createIndexes()
await Post.createIndexes()

// Or via mongosh:
mongosh "[CONNECTION_STRING]"
> use mydb
> db.users.createIndex({ email: 1 }, { unique: true })
```

### Atlas Search (full-text, optional but recommended over regex)
Atlas → Your Cluster → Search → Create Search Index → select collection → default config works for most cases

### Environment Variables
```env
MONGODB_URI="mongodb+srv://[USER]:[PASSWORD]@[CLUSTER].mongodb.net/[DB]?retryWrites=true&w=majority"
```

---

## Firebase Firestore

### Provision
1. Go to [console.firebase.google.com](https://console.firebase.google.com) → Add project
2. Firestore Database → Create database → Start in production mode → select region
3. Authentication → Sign-in method → enable needed providers (Email, Google, etc.)

### Get Config
Project Settings → Your apps → Add app (Web) → copy firebaseConfig object

### Deploy Security Rules
```bash
npm install -g firebase-tools
firebase login
firebase init firestore   # in your project root
# edit firestore.rules
firebase deploy --only firestore:rules
firebase deploy --only firestore:indexes
```

### Environment Variables
```env
# Public (safe to expose in client-side code)
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=[PROJECT-ID].firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=[PROJECT-ID]
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=[PROJECT-ID].appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=

# Server-side only (NEVER expose client-side)
FIREBASE_SERVICE_ACCOUNT_KEY=[JSON string of service account key]
```

### Get Service Account Key (for server-side admin SDK)
Project Settings → Service accounts → Generate new private key → download JSON → stringify it for env var:
```bash
cat firebase-service-account.json | tr -d '\n'
# paste the output as FIREBASE_SERVICE_ACCOUNT_KEY value
```

---

## Railway (PostgreSQL — great for quick deploys)

### Provision
1. Go to [railway.app](https://railway.app) → New Project → Add PostgreSQL
2. Connection string available immediately under Variables tab

### Get Connection String
Service → Variables → `DATABASE_URL` (auto-set)

### Apply Schema
```bash
# Railway CLI
railway run psql -f schema.sql

# Or via GUI: Open → psql → paste DDL
```

### Deploy App + DB Together
```bash
railway init
railway add --plugin postgresql
railway up
```

---

## AWS (RDS PostgreSQL — production / enterprise)

### Provision via Console
1. RDS → Create database → PostgreSQL → Free tier or production template
2. Set DB identifier, master username, master password
3. VPC settings: ensure your app can reach the DB (same VPC or public access for dev)
4. Enable automated backups

### Provision via Terraform (infrastructure as code)
```hcl
resource "aws_db_instance" "postgres" {
  identifier        = "myapp-db"
  engine            = "postgres"
  engine_version    = "15.4"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  db_name           = "myapp"
  username          = var.db_username
  password          = var.db_password
  skip_final_snapshot = true
  publicly_accessible = false
  vpc_security_group_ids = [aws_security_group.db.id]
}
```

### Connection String
```
postgresql://[USER]:[PASSWORD]@[ENDPOINT]:5432/[DB]?sslmode=require
```

---

## PlanetScale (Serverless MySQL)

### Provision
1. Go to [planetscale.com](https://planetscale.com) → New database
2. Choose region → Create
3. Create a password: Settings → Passwords → New password

### Connection String
```
mysql://[USERNAME]:[PASSWORD]@[HOST].psdb.cloud/[DATABASE]?sslaccept=strict
```

### Schema Branches (key workflow)
```bash
npm install -g pscale
pscale auth login
pscale branch create mydb my-feature        # create schema branch
pscale connect mydb my-feature --port 3309  # connect locally
# apply schema changes
pscale deploy-request create mydb my-feature  # PR for schema change
pscale deploy-request deploy mydb [NUMBER]    # merge to main
```

### Prisma + PlanetScale
```prisma
datasource db {
  provider     = "mysql"
  url          = env("DATABASE_URL")
  relationMode = "prisma"  // PlanetScale doesn't support FK constraints
}
```

---

## Redis via Upstash (serverless Redis)

### Provision
1. Go to [upstash.com](https://upstash.com) → Create database → Redis
2. Choose region and plan (free: 10k commands/day)
3. Copy REST URL and token

### Environment Variables
```env
UPSTASH_REDIS_REST_URL="https://[HOST].upstash.io"
UPSTASH_REDIS_REST_TOKEN="[TOKEN]"
```

### Usage (with @upstash/redis)
```bash
npm install @upstash/redis
```
```typescript
import { Redis } from '@upstash/redis'

const redis = new Redis({
  url: process.env.UPSTASH_REDIS_REST_URL!,
  token: process.env.UPSTASH_REDIS_REST_TOKEN!,
})

// Cache example
await redis.set('user:123', JSON.stringify(user), { ex: 3600 }) // TTL 1hr
const cached = await redis.get('user:123')

// Rate limiting
const requests = await redis.incr(`ratelimit:${ip}`)
await redis.expire(`ratelimit:${ip}`, 60)
if (requests > 100) return new Response('Too many requests', { status: 429 })
```

---

## SETUP.md Template

Use this template when generating the SETUP.md for the Execution Package:

```markdown
# Database Setup Guide

## Prerequisites
- Node.js 18+ (or relevant runtime)
- [DB CLI if needed]

## Step 1 — Provision the Database
[Specific steps for the chosen DB — copy from the relevant section above]

## Step 2 — Install Dependencies
\`\`\`bash
npm install [packages]
\`\`\`

## Step 3 — Configure Environment Variables
Copy `.env.example` to `.env.local` and fill in your values:
\`\`\`bash
cp .env.example .env.local
\`\`\`

[List each variable and where to find it]

## Step 4 — Run Migrations / Apply Schema
\`\`\`bash
[exact commands]
\`\`\`

## Step 5 — Verify Connection
\`\`\`bash
[test command or code snippet]
\`\`\`

## Step 6 — Seed Initial Data (optional)
\`\`\`bash
[seed command if applicable]
\`\`\`

## Troubleshooting

**Connection refused**: Check that DB is running, IP is whitelisted, SSL mode matches
**Migrations fail**: Ensure DIRECT_URL is set (not pooled URL) for migration commands
**Too many connections**: Switch to pooled connection URL, reduce pool max
```
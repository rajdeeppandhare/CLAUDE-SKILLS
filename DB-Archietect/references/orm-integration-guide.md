# 🧠 Memory Map Schema Reference

This file defines every section of the exported Memory Map and guidance on how to fill each one accurately.

---

## Section: Session Goal
**Purpose:** A 1–3 sentence summary of what the user was trying to accomplish.
**Tips:**
- Write it as if explaining to a new colleague who just joined the project
- Include the "why" if it was stated (e.g. "building a login flow for a SaaS MVP")
- Don't include steps taken — just the goal

---
# ORM Integration Guide

ORM selection, setup commands, connection patterns, and migration workflows per language and framework. Load this when deciding which ORM to recommend or when writing integration code.

---

## JavaScript / TypeScript

### Prisma (recommended for most JS/TS apps)

**Best with:** Next.js, Express, Fastify, NestJS, Remix, SvelteKit
**Supports:** PostgreSQL, MySQL, SQLite, MongoDB, CockroachDB, SQL Server

**Install:**
```bash
npm install prisma @prisma/client
npx prisma init
```

**Basic schema.prisma:**
```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  directUrl = env("DIRECT_URL") // for Supabase/pooled connections
}

generator client {
  provider = "prisma-client-js"
}
```

**Migration workflow:**
```bash
npx prisma migrate dev --name init          # create + apply migration (dev)
npx prisma migrate deploy                   # apply migrations (production)
npx prisma db push                          # push schema without migration file (prototyping)
npx prisma studio                           # GUI to browse data
npx prisma generate                         # regenerate client after schema change
```

**Next.js singleton pattern:**
```typescript
// lib/prisma.ts
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient }

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' ? ['query', 'error', 'warn'] : ['error'],
  })

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma
```

**Common query patterns:**
```typescript
// Find with relations
const user = await prisma.user.findUnique({
  where: { id: userId },
  include: { sessions: true, orgMemberships: { include: { org: true } } }
})

// Paginated list
const posts = await prisma.post.findMany({
  where: { status: 'published' },
  orderBy: { publishedAt: 'desc' },
  take: 20,
  skip: (page - 1) * 20,
})

// Upsert
const user = await prisma.user.upsert({
  where: { email },
  update: { name, updatedAt: new Date() },
  create: { email, name },
})

// Transaction
const result = await prisma.$transaction(async (tx) => {
  const order = await tx.order.create({ data: orderData })
  await tx.product.update({ where: { id: productId }, data: { stock: { decrement: quantity } } })
  return order
})
```

---

### Drizzle ORM (lightweight, type-safe, SQL-first)

**Best with:** Serverless (Vercel, Cloudflare Workers), Next.js App Router, edge runtimes
**Supports:** PostgreSQL, MySQL, SQLite

**Install:**
```bash
npm install drizzle-orm @neondatabase/serverless  # for Neon
npm install drizzle-orm postgres                  # for standard Postgres
npm install -D drizzle-kit
```

**Schema definition:**
```typescript
// db/schema.ts
import { pgTable, uuid, text, timestamp, boolean } from 'drizzle-orm/pg-core'

export const users = pgTable('users', {
  id:        uuid('id').primaryKey().defaultRandom(),
  email:     text('email').notNull().unique(),
  name:      text('name'),
  createdAt: timestamp('created_at').defaultNow().notNull(),
})

export const posts = pgTable('posts', {
  id:       uuid('id').primaryKey().defaultRandom(),
  authorId: uuid('author_id').notNull().references(() => users.id, { onDelete: 'cascade' }),
  title:    text('title').notNull(),
  status:   text('status', { enum: ['draft','published'] }).default('draft'),
})
```

**Migration workflow:**
```bash
npx drizzle-kit generate:pg   # generate SQL migration
npx drizzle-kit push:pg       # push directly (dev only)
npx drizzle-kit studio        # GUI
```

**Connection (Neon serverless):**
```typescript
// db/index.ts
import { neon } from '@neondatabase/serverless'
import { drizzle } from 'drizzle-orm/neon-http'
import * as schema from './schema'

const sql = neon(process.env.DATABASE_URL!)
export const db = drizzle(sql, { schema })
```

---

### Mongoose (MongoDB)

**Best with:** Express, Fastify, any Node.js app on MongoDB

**Install:**
```bash
npm install mongoose
```

**Connection singleton (Next.js):**
```javascript
// lib/db.js
import mongoose from 'mongoose'

const MONGODB_URI = process.env.MONGODB_URI
if (!MONGODB_URI) throw new Error('MONGODB_URI not defined')

let cached = global.mongoose ?? { conn: null, promise: null }
if (!global.mongoose) global.mongoose = cached

export async function connectDB() {
  if (cached.conn) return cached.conn
  if (!cached.promise) {
    cached.promise = mongoose.connect(MONGODB_URI, { bufferCommands: false })
  }
  try {
    cached.conn = await cached.promise
  } catch (e) {
    cached.promise = null
    throw e
  }
  return cached.conn
}
```

**Schema with validation:**
```javascript
// models/User.js
import mongoose, { Schema, models, model } from 'mongoose'

const userSchema = new Schema({
  email:    { type: String, required: true, unique: true, lowercase: true, trim: true },
  name:     { type: String, trim: true, maxlength: 100 },
  role:     { type: String, enum: ['user','admin'], default: 'user' },
  isActive: { type: Boolean, default: true },
}, {
  timestamps: true,
  toJSON: { virtuals: true },
})

export const User = models.User ?? model('User', userSchema)
```

---

### TypeORM (NestJS / enterprise)

**Best with:** NestJS, enterprise Node.js apps, teams from Java/C# backgrounds
**Supports:** PostgreSQL, MySQL, SQLite, MongoDB, SQL Server, Oracle

**Install:**
```bash
npm install typeorm reflect-metadata pg
```

**Entity:**
```typescript
// entities/User.entity.ts
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn, OneToMany } from 'typeorm'

@Entity('users')
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string

  @Column({ unique: true })
  email: string

  @Column({ nullable: true })
  name: string

  @CreateDateColumn() createdAt: Date
  @UpdateDateColumn() updatedAt: Date
}
```

**Migration:**
```bash
npx typeorm migration:generate src/migrations/Init -d src/data-source.ts
npx typeorm migration:run -d src/data-source.ts
```

---

## Python

### SQLAlchemy 2.x (recommended for most Python apps)

**Best with:** FastAPI, Flask, Django (via SQLAlchemy instead of ORM), any Python app
**Supports:** PostgreSQL, MySQL, SQLite, Oracle, MSSQL

**Install:**
```bash
pip install sqlalchemy asyncpg alembic  # asyncpg for async, psycopg2-binary for sync
```

**Async model definition (FastAPI):**
```python
# models/user.py
from sqlalchemy import String, DateTime, func
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column
from datetime import datetime
import uuid

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id:         Mapped[str]      = mapped_column(String, primary_key=True, default=lambda: str(uuid.uuid4()))
    email:      Mapped[str]      = mapped_column(String, unique=True, nullable=False, index=True)
    name:       Mapped[str|None] = mapped_column(String(100))
    created_at: Mapped[datetime] = mapped_column(DateTime(timezone=True), server_default=func.now())
```

**Async session (FastAPI):**
```python
# db/session.py
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession, async_sessionmaker

engine = create_async_engine(settings.DATABASE_URL, echo=False, pool_size=20, max_overflow=0)
AsyncSessionLocal = async_sessionmaker(engine, expire_on_commit=False)

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session
```

**Alembic migrations:**
```bash
alembic init alembic
alembic revision --autogenerate -m "init"
alembic upgrade head
```

---

### Tortoise ORM (async-first Python)

**Best with:** FastAPI, Sanic — when you want Django-style ORM with async
**Supports:** PostgreSQL, MySQL, SQLite

**Install:**
```bash
pip install tortoise-orm aerich asyncpg
```

**Model:**
```python
from tortoise import fields, models

class User(models.Model):
    id         = fields.UUIDField(pk=True)
    email      = fields.CharField(max_length=255, unique=True, index=True)
    name       = fields.CharField(max_length=100, null=True)
    created_at = fields.DatetimeField(auto_now_add=True)
    updated_at = fields.DatetimeField(auto_now=True)

    class Meta:
        table = "users"
```

---

## Go

### GORM

**Best with:** Gin, Echo, Fiber — most Go web apps
**Supports:** PostgreSQL, MySQL, SQLite, SQL Server

**Install:**
```bash
go get gorm.io/gorm gorm.io/driver/postgres
```

**Model + connection:**
```go
// models/user.go
type User struct {
    gorm.Model
    ID        uuid.UUID `gorm:"type:uuid;primaryKey;default:gen_random_uuid()"`
    Email     string    `gorm:"uniqueIndex;not null"`
    Name      string
    Role      string    `gorm:"default:user"`
}

// db/db.go
func Connect() *gorm.DB {
    db, err := gorm.Open(postgres.Open(os.Getenv("DATABASE_URL")), &gorm.Config{})
    if err != nil { log.Fatal("DB connection failed:", err) }
    db.AutoMigrate(&User{}, &Post{}) // use proper migrations in prod
    return db
}
```

---

## PHP

### Eloquent (Laravel)

**Built into Laravel** — no install needed.
**Supports:** PostgreSQL, MySQL, SQLite, SQL Server

**Model:**
```php
// app/Models/User.php
class User extends Model {
    protected $fillable = ['name', 'email', 'password'];
    protected $hidden   = ['password', 'remember_token'];
    protected $casts    = ['email_verified_at' => 'datetime'];

    public function posts(): HasMany {
        return $this->hasMany(Post::class);
    }
}
```

**Migration:**
```bash
php artisan make:migration create_users_table
php artisan migrate
php artisan migrate:rollback
```

---

## ORM Selection Summary

| Stack | Recommended ORM | Alternative |
|---|---|---|
| Next.js / TypeScript + Postgres | Prisma | Drizzle (serverless) |
| Next.js / TypeScript + Mongo | Mongoose | — |
| Express / TypeScript | Prisma or TypeORM | Drizzle |
| NestJS | TypeORM | Prisma |
| Serverless (Vercel edge) | Drizzle | Prisma (with connection pooler) |
| FastAPI / Flask | SQLAlchemy 2.x | Tortoise ORM |
| Django | Django ORM (built-in) | SQLAlchemy |
| Go + Gin/Echo | GORM | sqlx (raw SQL) |
| Laravel | Eloquent (built-in) | — |
| Rails | ActiveRecord (built-in) | — |
## Section: User Context
**Purpose:** Relevant background about the user that shapes how to respond.
**Capture:**
- Role / job title if mentioned
- Tech stack or language preferences
- Skill level (beginner, intermediate, expert) inferred from language used
- Constraints (e.g. "can't use paid APIs", "must work on Windows", "deadline tomorrow")
- Communication preferences (e.g. prefers short answers, wants code only)

---

## Section: Current Status
**Purpose:** The exact state of the work at the moment of export.
**Tips:**
- "Where we are" = one sentence, present tense
- "Next planned step" = the literal next action that was about to happen
- "Confidence level" = rough % complete or a qualitative label (stuck / in progress / nearly done)

---

## Section: What Was Accomplished
**Purpose:** Ordered list of completed steps, working code, confirmed decisions.
**Tips:**
- Number each item
- Use past tense ("Created X", "Fixed Y", "Decided to use Z")
- Include things that were *confirmed working*, not just attempted

---

## Section: Issues & Resolutions
**Purpose:** A table of every significant problem that came up.

| Column | What to put |
|--------|------------|
| Issue | Short description of the problem |
| Status | ✅ Resolved / ❌ Unresolved / ⚠️ Workaround |
| Resolution / Notes | What fixed it, or current best theory if unresolved |

**Tips:**
- Even minor blockers worth noting (e.g. wrong package version)
- Unresolved issues are the most important to capture

---

## Section: Code / Artifacts Produced
**Purpose:** Every file created or significantly modified.
**Tips:**
- Include full code if under ~150 lines
- For longer files, include: file purpose, the most-changed functions/sections, any TODOs
- Always include filename with path if known
- If multiple versions were tried, include the final working one only (unless the difference matters)

---

## Section: Key Decisions
**Purpose:** Choices that shaped the direction of the work.
**Tips:**
- Format: "Decided to use X instead of Y because Z"
- Include architectural decisions, library choices, approach selections
- If the user explicitly rejected something, note it here

---

## Section: Dead Ends & Things That Failed
**Purpose:** Approaches that were tried and abandoned — critical to avoid repeating.
**Tips:**
- Be specific about *why* something failed, not just that it failed
- Include error messages if they were meaningful
- This section is often the most valuable for resumption

---

## Section: Environment & Setup
**Purpose:** Everything needed to reproduce the working state.
**Checklist:**
- [ ] OS / platform
- [ ] Language + runtime version (e.g. Python 3.11, Node 20)
- [ ] Package manager + key dependencies with versions
- [ ] Environment variables needed (names only, never values)
- [ ] File paths / directory structure if relevant
- [ ] Any setup commands that were run

---

## Section: Conversation Highlights
**Purpose:** 3–7 exchanges that carried the most signal.
**What qualifies:**
- A decision being made
- A key constraint being revealed
- An important error message
- A clarification that changed direction
- The user correcting Claude

**What doesn't qualify:**
- Greetings
- "That looks good"
- Summaries Claude provided (already captured elsewhere)

---

## Section: Resume Instructions
**Purpose:** The exact prompt the user should give the next Claude instance.
**Template:**
```
"I'm resuming a session. Here's the memory map: [uploaded file].
Please read it fully, confirm you understand the current state,
then continue from: [NEXT PLANNED STEP]."
```
Customize the last line with the literal next step from the Current Status section.

---

## Quality Checklist Before Export

- [ ] Would a stranger understand the goal from just this file?
- [ ] Is every unresolved issue documented?
- [ ] Is all produced code included or summarized?
- [ ] Are dead ends clearly documented?
- [ ] Does the Resume Instructions section have the correct next step?
- [ ] Are environment details complete enough to reproduce the setup?

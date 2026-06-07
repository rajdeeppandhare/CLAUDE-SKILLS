# Schema Patterns Library

Battle-tested schemas for the most common application domains. Load this when designing schemas — pick the relevant pattern and adapt to the codebase.

---

## Pattern: Authentication & Users

### PostgreSQL DDL
```sql
CREATE EXTENSION IF NOT EXISTS "pgcrypto";

CREATE TABLE users (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email       TEXT UNIQUE NOT NULL,
  name        TEXT,
  avatar_url  TEXT,
  role        TEXT NOT NULL DEFAULT 'user' CHECK (role IN ('user','admin','moderator')),
  email_verified_at TIMESTAMPTZ,
  created_at  TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at  TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE sessions (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id    UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token      TEXT UNIQUE NOT NULL,
  expires_at TIMESTAMPTZ NOT NULL,
  ip_address INET,
  user_agent TEXT,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE password_reset_tokens (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id    UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token      TEXT UNIQUE NOT NULL,
  expires_at TIMESTAMPTZ NOT NULL,
  used_at    TIMESTAMPTZ,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_sessions_user_id  ON sessions(user_id);
CREATE INDEX idx_sessions_token    ON sessions(token);
CREATE INDEX idx_sessions_expires  ON sessions(expires_at);
```

### Prisma Schema
```prisma
model User {
  id               String    @id @default(uuid())
  email            String    @unique
  name             String?
  avatarUrl        String?
  role             Role      @default(USER)
  emailVerifiedAt  DateTime?
  sessions         Session[]
  createdAt        DateTime  @default(now())
  updatedAt        DateTime  @updatedAt
}

model Session {
  id        String   @id @default(uuid())
  userId    String
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  token     String   @unique
  expiresAt DateTime
  ipAddress String?
  userAgent String?
  createdAt DateTime @default(now())
  @@index([userId])
}

enum Role { USER ADMIN MODERATOR }
```

### MongoDB (Mongoose)
```javascript
const userSchema = new Schema({
  email:           { type: String, required: true, unique: true, lowercase: true, trim: true },
  name:            { type: String, trim: true },
  avatarUrl:       { type: String },
  role:            { type: String, enum: ['user', 'admin', 'moderator'], default: 'user' },
  passwordHash:    { type: String, select: false },
  emailVerifiedAt: { type: Date },
}, { timestamps: true });

userSchema.index({ email: 1 });
```

---

## Pattern: SaaS Multi-Tenancy (Organisations + Members)

### PostgreSQL DDL
```sql
CREATE TABLE organisations (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name       TEXT NOT NULL,
  slug       TEXT UNIQUE NOT NULL,
  plan       TEXT NOT NULL DEFAULT 'free' CHECK (plan IN ('free','starter','pro','enterprise')),
  logo_url   TEXT,
  settings   JSONB DEFAULT '{}',
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE org_members (
  id        UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  org_id    UUID NOT NULL REFERENCES organisations(id) ON DELETE CASCADE,
  user_id   UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  role      TEXT NOT NULL DEFAULT 'member' CHECK (role IN ('owner','admin','member','viewer')),
  joined_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  UNIQUE(org_id, user_id)
);

CREATE TABLE org_invites (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  org_id     UUID NOT NULL REFERENCES organisations(id) ON DELETE CASCADE,
  email      TEXT NOT NULL,
  role       TEXT NOT NULL DEFAULT 'member',
  token      TEXT UNIQUE NOT NULL,
  expires_at TIMESTAMPTZ NOT NULL,
  accepted_at TIMESTAMPTZ,
  invited_by UUID REFERENCES users(id),
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_org_members_org  ON org_members(org_id);
CREATE INDEX idx_org_members_user ON org_members(user_id);

-- RLS for multi-tenant isolation (Supabase)
ALTER TABLE organisations ENABLE ROW LEVEL SECURITY;
CREATE POLICY org_member_access ON organisations
  USING (id IN (
    SELECT org_id FROM org_members
    WHERE user_id = auth.uid()
  ));
```

### Prisma
```prisma
model Organisation {
  id        String      @id @default(uuid())
  name      String
  slug      String      @unique
  plan      OrgPlan     @default(FREE)
  logoUrl   String?
  settings  Json        @default("{}")
  members   OrgMember[]
  invites   OrgInvite[]
  createdAt DateTime    @default(now())
  updatedAt DateTime    @updatedAt
}

model OrgMember {
  id       String       @id @default(uuid())
  orgId    String
  org      Organisation @relation(fields: [orgId], references: [id], onDelete: Cascade)
  userId   String
  user     User         @relation(fields: [userId], references: [id], onDelete: Cascade)
  role     OrgRole      @default(MEMBER)
  joinedAt DateTime     @default(now())
  @@unique([orgId, userId])
  @@index([orgId])
  @@index([userId])
}

enum OrgPlan  { FREE STARTER PRO ENTERPRISE }
enum OrgRole  { OWNER ADMIN MEMBER VIEWER }
```

---

## Pattern: E-Commerce (Products, Orders, Line Items)

### PostgreSQL DDL
```sql
CREATE TABLE categories (
  id        UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name      TEXT NOT NULL,
  slug      TEXT UNIQUE NOT NULL,
  parent_id UUID REFERENCES categories(id)
);

CREATE TABLE products (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  category_id   UUID REFERENCES categories(id),
  name          TEXT NOT NULL,
  description   TEXT,
  price_cents   INT NOT NULL CHECK (price_cents >= 0),
  currency      TEXT NOT NULL DEFAULT 'USD',
  stock         INT NOT NULL DEFAULT 0 CHECK (stock >= 0),
  sku           TEXT UNIQUE,
  images        TEXT[],
  metadata      JSONB DEFAULT '{}',
  is_active     BOOLEAN DEFAULT true,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE orders (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id       UUID NOT NULL REFERENCES users(id),
  status        TEXT NOT NULL DEFAULT 'pending'
                  CHECK (status IN ('pending','paid','processing','shipped','delivered','cancelled','refunded')),
  total_cents   INT NOT NULL,
  currency      TEXT NOT NULL DEFAULT 'USD',
  shipping_addr JSONB,
  billing_addr  JSONB,
  notes         TEXT,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE order_items (
  id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  order_id    UUID NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
  product_id  UUID NOT NULL REFERENCES products(id),
  quantity    INT NOT NULL CHECK (quantity > 0),
  unit_price  INT NOT NULL,
  total_price INT GENERATED ALWAYS AS (quantity * unit_price) STORED
);

CREATE INDEX idx_products_category   ON products(category_id);
CREATE INDEX idx_products_active     ON products(is_active);
CREATE INDEX idx_orders_user         ON orders(user_id);
CREATE INDEX idx_orders_status       ON orders(status);
CREATE INDEX idx_order_items_order   ON order_items(order_id);
CREATE INDEX idx_order_items_product ON order_items(product_id);
```

---

## Pattern: Content / CMS (Posts, Tags, Authors)

### PostgreSQL DDL
```sql
CREATE TABLE posts (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  author_id     UUID NOT NULL REFERENCES users(id),
  title         TEXT NOT NULL,
  slug          TEXT UNIQUE NOT NULL,
  content       TEXT,
  excerpt       TEXT,
  cover_url     TEXT,
  status        TEXT NOT NULL DEFAULT 'draft'
                  CHECK (status IN ('draft','published','archived')),
  published_at  TIMESTAMPTZ,
  reading_time  INT, -- minutes
  view_count    INT NOT NULL DEFAULT 0,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT now(),
  search_vector TSVECTOR GENERATED ALWAYS AS (
    to_tsvector('english', coalesce(title,'') || ' ' || coalesce(excerpt,'') || ' ' || coalesce(content,''))
  ) STORED
);

CREATE TABLE tags (
  id   UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT UNIQUE NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  color TEXT
);

CREATE TABLE post_tags (
  post_id UUID REFERENCES posts(id) ON DELETE CASCADE,
  tag_id  UUID REFERENCES tags(id) ON DELETE CASCADE,
  PRIMARY KEY (post_id, tag_id)
);

CREATE INDEX idx_posts_author   ON posts(author_id);
CREATE INDEX idx_posts_status   ON posts(status, published_at DESC);
CREATE INDEX idx_posts_search   ON posts USING GIN(search_vector);
```

### MongoDB (Mongoose) — for flexible CMS
```javascript
const postSchema = new Schema({
  authorId:    { type: Schema.Types.ObjectId, ref: 'User', required: true, index: true },
  title:       { type: String, required: true, trim: true },
  slug:        { type: String, required: true, unique: true },
  content:     { type: String },
  excerpt:     { type: String },
  coverUrl:    { type: String },
  tags:        [{ type: String, trim: true }],
  status:      { type: String, enum: ['draft','published','archived'], default: 'draft', index: true },
  publishedAt: { type: Date },
  metadata:    { type: Schema.Types.Mixed }, // flexible extra fields
}, { timestamps: true });

postSchema.index({ title: 'text', content: 'text', excerpt: 'text' }); // use Atlas Search in prod
```

---

## Pattern: Real-Time Chat / Messaging

### PostgreSQL (for history + search)
```sql
CREATE TABLE channels (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name       TEXT,
  type       TEXT NOT NULL DEFAULT 'group' CHECK (type IN ('direct','group','announcement')),
  created_by UUID REFERENCES users(id),
  created_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE channel_members (
  channel_id    UUID REFERENCES channels(id) ON DELETE CASCADE,
  user_id       UUID REFERENCES users(id) ON DELETE CASCADE,
  last_read_at  TIMESTAMPTZ DEFAULT now(),
  joined_at     TIMESTAMPTZ DEFAULT now(),
  PRIMARY KEY (channel_id, user_id)
);

CREATE TABLE messages (
  id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  channel_id   UUID NOT NULL REFERENCES channels(id) ON DELETE CASCADE,
  sender_id    UUID NOT NULL REFERENCES users(id),
  content      TEXT NOT NULL,
  reply_to_id  UUID REFERENCES messages(id),
  edited_at    TIMESTAMPTZ,
  deleted_at   TIMESTAMPTZ,
  created_at   TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_messages_channel ON messages(channel_id, created_at DESC);
CREATE INDEX idx_messages_sender  ON messages(sender_id);
```

### Firestore (for real-time chat — preferred)
```
/channels/{channelId}
  - name: string
  - type: 'direct' | 'group'
  - memberIds: string[]         ← denormalised for security rules
  - lastMessage: string         ← denormalised for channel list preview
  - lastMessageAt: timestamp
  - createdAt: timestamp

/channels/{channelId}/messages/{messageId}
  - senderId: string
  - senderName: string          ← denormalised (avoid extra read per message)
  - content: string
  - replyToId: string | null
  - createdAt: timestamp

/users/{userId}/channelMeta/{channelId}
  - lastReadAt: timestamp        ← for unread count calculation
```

---

## Pattern: File / Media Uploads

### PostgreSQL DDL
```sql
CREATE TABLE files (
  id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  uploader_id    UUID NOT NULL REFERENCES users(id),
  filename       TEXT NOT NULL,        -- stored name (UUID-based)
  original_name  TEXT NOT NULL,        -- user's original filename
  mime_type      TEXT NOT NULL,
  size_bytes     BIGINT NOT NULL,
  storage_key    TEXT NOT NULL UNIQUE, -- path in S3/Supabase Storage
  public_url     TEXT NOT NULL,
  entity_type    TEXT,                 -- 'post' | 'user' | 'product' (polymorphic)
  entity_id      UUID,
  created_at     TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_files_uploader ON files(uploader_id);
CREATE INDEX idx_files_entity   ON files(entity_type, entity_id);
```

---

## Pattern: Notifications

### PostgreSQL DDL
```sql
CREATE TABLE notifications (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id    UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  type       TEXT NOT NULL,          -- 'mention' | 'reply' | 'follow' | 'order_update' | 'system'
  title      TEXT NOT NULL,
  body       TEXT,
  action_url TEXT,
  data       JSONB DEFAULT '{}',     -- contextual data (post_id, sender_id, order_id, etc.)
  read_at    TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_notifications_user   ON notifications(user_id, created_at DESC);
CREATE INDEX idx_notifications_unread ON notifications(user_id) WHERE read_at IS NULL;
```

---

## Pattern: Payments & Subscriptions (Stripe)

### PostgreSQL DDL
```sql
CREATE TABLE subscriptions (
  id                     UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id                UUID NOT NULL REFERENCES users(id),
  org_id                 UUID REFERENCES organisations(id),
  stripe_customer_id     TEXT UNIQUE NOT NULL,
  stripe_subscription_id TEXT UNIQUE,
  plan                   TEXT NOT NULL DEFAULT 'free',
  status                 TEXT NOT NULL DEFAULT 'active'
                           CHECK (status IN ('active','cancelled','past_due','trialing','incomplete')),
  trial_ends_at          TIMESTAMPTZ,
  current_period_start   TIMESTAMPTZ,
  current_period_end     TIMESTAMPTZ,
  cancel_at_period_end   BOOLEAN DEFAULT false,
  created_at             TIMESTAMPTZ DEFAULT now(),
  updated_at             TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE payments (
  id                 UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id            UUID NOT NULL REFERENCES users(id),
  stripe_payment_id  TEXT UNIQUE NOT NULL,
  amount_cents       INT NOT NULL,
  currency           TEXT NOT NULL DEFAULT 'usd',
  status             TEXT NOT NULL,   -- 'succeeded' | 'failed' | 'refunded'
  description        TEXT,
  invoice_url        TEXT,
  created_at         TIMESTAMPTZ DEFAULT now()
);

CREATE INDEX idx_subscriptions_user   ON subscriptions(user_id);
CREATE INDEX idx_subscriptions_stripe ON subscriptions(stripe_customer_id);
CREATE INDEX idx_payments_user        ON payments(user_id);
```

---

## Pattern: Activity / Audit Log

### PostgreSQL DDL
```sql
CREATE TABLE activity_log (
  id          BIGSERIAL PRIMARY KEY,           -- serial for append-only, high volume
  actor_id    UUID REFERENCES users(id),
  action      TEXT NOT NULL,                   -- 'created' | 'updated' | 'deleted' | 'login' etc.
  entity_type TEXT NOT NULL,                   -- 'post' | 'user' | 'order' etc.
  entity_id   UUID,
  before      JSONB,                           -- state before change
  after       JSONB,                           -- state after change
  ip_address  INET,
  user_agent  TEXT,
  created_at  TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE INDEX idx_activity_actor    ON activity_log(actor_id, created_at DESC);
CREATE INDEX idx_activity_entity   ON activity_log(entity_type, entity_id, created_at DESC);
-- Consider TimescaleDB hypertable for high-volume audit logs
```
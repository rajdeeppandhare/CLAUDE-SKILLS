# Feature Blueprints Library

> Pre-built architecture blueprints for the most common feature integrations. Load during Phase 3 (Architecture Design) and Phase 5 (Blueprint) when the requested feature matches a category below.

---

## Table of Contents

1. [Stripe Subscription Billing](#1-stripe-subscription-billing)
2. [OAuth Authentication (Google / GitHub)](#2-oauth-authentication-google--github)
3. [Role-Based Access Control (RBAC)](#3-role-based-access-control-rbac)
4. [Real-Time Notifications (WebSockets)](#4-real-time-notifications-websockets)
5. [Team & Organisation Collaboration](#5-team--organisation-collaboration)
6. [File Uploads (S3 / Cloud Storage)](#6-file-uploads-s3--cloud-storage)
7. [Transactional Email](#7-transactional-email)
8. [Full-Text Search](#8-full-text-search)
9. [AI / LLM Integration](#9-ai--llm-integration)
10. [Admin Panel & Audit Logs](#10-admin-panel--audit-logs)

---

## 1. Stripe Subscription Billing

### Key Entities
- `customers` — Stripe customer ID linked to user
- `subscriptions` — plan, status, period dates, Stripe subscription ID
- `billing_events` — idempotency log of all webhook events

### Core Services
- `BillingService` — createCustomer, createSubscription, cancelSubscription, updatePlan
- `WebhookHandler` — verify Stripe signature, process events idempotently

### Critical Webhook Events to Handle
| Event | Action |
|---|---|
| `checkout.session.completed` | Activate subscription |
| `customer.subscription.updated` | Sync plan changes |
| `customer.subscription.deleted` | Downgrade to free |
| `invoice.payment_failed` | Flag account, notify user |
| `invoice.payment_succeeded` | Extend billing period |

### Idempotency Pattern
```sql
CREATE TABLE billing_events (
  id UUID PRIMARY KEY,
  stripe_event_id VARCHAR UNIQUE NOT NULL,  -- idempotency key
  event_type VARCHAR NOT NULL,
  processed_at TIMESTAMPTZ,
  payload JSONB
);
```
Before processing: `SELECT id FROM billing_events WHERE stripe_event_id = ?`
If exists → skip. If not → process + insert.

### Recommended Build Order
1. Extend user/auth schema (add `subscription_tier`, `stripe_customer_id`)
2. Create BillingService wrapper
3. Build webhook endpoint with signature verification
4. Add billing_events idempotency table
5. Build checkout flow (frontend → backend → Stripe)
6. Build customer portal link (for self-service upgrades/cancellations)

### Risks to Flag
- R-PAY-01: Webhook idempotency (always)
- R-AUTH-01: Subscription tiers need role model extension
- R-DB-04: Subscription create + user update must be in a transaction
- R-CONC-01: Race condition on concurrent upgrade requests

---

## 2. OAuth Authentication (Google / GitHub)

### Flow (Server-Side)
```
User clicks "Login with Google"
→ Redirect to /api/auth/google
→ Redirect to Google OAuth consent screen
→ Google redirects to /api/auth/google/callback?code=...
→ Exchange code for tokens
→ Fetch user profile from Google
→ Upsert user in DB (find by email or google_id)
→ Create session / issue JWT
→ Redirect to app
```

### Key DB Fields to Add
```sql
ALTER TABLE users ADD COLUMN google_id VARCHAR UNIQUE;
ALTER TABLE users ADD COLUMN github_id VARCHAR UNIQUE;
ALTER TABLE users ADD COLUMN avatar_url VARCHAR;
ALTER TABLE users ADD COLUMN auth_provider VARCHAR DEFAULT 'email';
```

### Account Linking Logic
1. Check if Google email matches existing user — if yes, link accounts
2. If no existing user — create new user
3. Handle case where user already has password auth — offer to link

### Session Strategy
- If using JWT: embed `sub`, `email`, `role`, `org_id` in claims
- If using sessions: store `user_id`, `provider`, `org_id` in session store

### Risks to Flag
- R-AUTH-05: Request minimum OAuth scopes (profile + email only)
- R-AUTH-07: CSRF state parameter required in OAuth flow
- Existing password users: handle account linking carefully

---

## 3. Role-Based Access Control (RBAC)

### Schema Pattern
```sql
CREATE TABLE roles (
  id UUID PRIMARY KEY,
  name VARCHAR UNIQUE NOT NULL,  -- 'admin', 'editor', 'viewer'
  description TEXT
);

CREATE TABLE user_roles (
  user_id UUID REFERENCES users(id),
  role_id UUID REFERENCES roles(id),
  scope_type VARCHAR,  -- 'global', 'org', 'project'
  scope_id UUID,       -- NULL for global, org_id or project_id otherwise
  PRIMARY KEY (user_id, role_id, COALESCE(scope_id, '00000000-0000-0000-0000-000000000000'))
);
```

### Middleware Pattern
```typescript
// requirePermission('documents:write')
function requirePermission(permission: string) {
  return async (req, res, next) => {
    const hasPermission = await checkUserPermission(req.user.id, permission, req.params.orgId);
    if (!hasPermission) return res.status(403).json({ error: 'Forbidden' });
    next();
  };
}
```

### Permission String Convention
`resource:action` — e.g. `documents:read`, `billing:manage`, `members:invite`

### Risks to Flag
- R-AUTH-01: Migrate from `isAdmin: boolean` pattern — add migration + default role backfill
- R-AUTH-04: Horizontal privilege escalation — always scope permission checks by org/resource
- R-DEPLOY-02: Add role backfill migration for existing users

---

## 4. Real-Time Notifications (WebSockets)

### Architecture Options

| Option | Best For | Complexity |
|---|---|---|
| WebSockets (native / socket.io) | High-frequency, bidirectional | Medium |
| Server-Sent Events (SSE) | Server-push only, simpler | Low |
| Polling | Low traffic, simple setup | Very Low |
| Pusher / Ably | Managed, fastest to ship | Low (cost) |

### Notification Schema
```sql
CREATE TABLE notifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  type VARCHAR NOT NULL,          -- 'mention', 'assignment', 'billing'
  title VARCHAR NOT NULL,
  body TEXT,
  action_url VARCHAR,
  read_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_notifications_user_unread ON notifications(user_id, read_at) WHERE read_at IS NULL;
```

### Delivery Pattern
1. Action triggers notification creation (DB write)
2. Event emitted to notification service
3. WebSocket/SSE pushes to connected clients
4. Fallback: in-app bell icon polls `/api/notifications?unread=true`

### Risks to Flag
- R-DB-05: Index on `(user_id, read_at)` required
- R-DB-08: Paginate notification list (never return all)
- R-CONC-03: Multiple tabs open — handle duplicate delivery
- R-PERF-01: Send actual notification async (don't block the action that triggered it)

---

## 5. Team & Organisation Collaboration

### Core Schema
```sql
CREATE TABLE organisations (
  id UUID PRIMARY KEY,
  name VARCHAR NOT NULL,
  slug VARCHAR UNIQUE NOT NULL,
  plan VARCHAR DEFAULT 'free',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE org_members (
  org_id UUID REFERENCES organisations(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  role VARCHAR NOT NULL DEFAULT 'member',  -- 'owner', 'admin', 'member', 'viewer'
  joined_at TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (org_id, user_id)
);

CREATE TABLE invitations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  org_id UUID REFERENCES organisations(id) ON DELETE CASCADE,
  email VARCHAR NOT NULL,
  role VARCHAR NOT NULL DEFAULT 'member',
  token VARCHAR UNIQUE NOT NULL,
  invited_by UUID REFERENCES users(id),
  expires_at TIMESTAMPTZ NOT NULL,
  accepted_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Session Multi-Tenancy
Add `active_org_id` to session/JWT. On org switch: update session + reload data scoped to new org.

### Data Isolation Pattern
Every query for org-scoped resources must include:
```sql
WHERE org_id = $current_org_id AND ...
```
Never query resources without org scope once multi-tenancy is introduced.

### Risks to Flag
- R-AUTH-02: Session scope mismatch — org context must be in session
- R-AUTH-04: Horizontal privilege escalation — all queries must be org-scoped
- R-AUTH-07: Invitation token expiry and single-use enforcement
- R-DB-03: Cascade deletes — deleting org must clean up all child records

---

## 6. File Uploads (S3 / Cloud Storage)

### Recommended Pattern: Pre-signed URLs
```
Client → Request upload URL from backend
Backend → Generate S3 pre-signed URL (PUT, 5-min expiry, max size)
Client → Upload directly to S3 (no data through your server)
Client → Notify backend of completed upload
Backend → Verify file exists in S3, store metadata in DB
```

### File Metadata Schema
```sql
CREATE TABLE files (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  owner_id UUID NOT NULL REFERENCES users(id),
  org_id UUID REFERENCES organisations(id),
  storage_key VARCHAR NOT NULL UNIQUE,  -- S3 key (UUID-based, NOT original filename)
  original_filename VARCHAR NOT NULL,
  mime_type VARCHAR NOT NULL,
  size_bytes INTEGER NOT NULL,
  purpose VARCHAR,                      -- 'avatar', 'document', 'attachment'
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Access Pattern
For private files:
```
Client requests file → Backend verifies ownership → Generates signed GET URL (15-min expiry) → Redirect client to signed URL
```
Never expose direct S3 URLs for private files.

### Risks to Flag
- R-FILE-01: Enforce size limits in pre-signed URL AND application
- R-FILE-02: Validate MIME type server-side, not just extension
- R-FILE-03: Default bucket to PRIVATE; use signed URLs
- R-FILE-04: Always generate UUID-based storage key; never use original filename

---

## 7. Transactional Email

### Service Comparison
| Service | Best For |
|---|---|
| Resend | Modern apps, developer DX |
| SendGrid | Scale, templates |
| Postmark | Reliability, deliverability |
| AWS SES | Cost at scale |

### Email Queue Pattern
Never send email synchronously in request path. Always queue:
```
Action (signup, invite, payment) → Push job to queue → Email worker sends → Log result
```

### Email Event Schema
```sql
CREATE TABLE email_log (
  id UUID PRIMARY KEY,
  recipient VARCHAR NOT NULL,
  template VARCHAR NOT NULL,
  status VARCHAR DEFAULT 'queued',   -- 'queued', 'sent', 'delivered', 'bounced', 'failed'
  provider_message_id VARCHAR,
  sent_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Risks to Flag
- R-PERF-01: Never block request on email send — always async
- R-API-03: Email-triggered endpoints need rate limiting (invite spam)
- R-3P-01: Graceful degradation if email provider is down

---

## 8. Full-Text Search

### Option Selection Guide
| Option | Best For | Complexity |
|---|---|---|
| PostgreSQL `tsvector` | Existing Postgres, moderate scale | Low |
| Algolia | Fast setup, great relevance, managed | Low (cost) |
| Typesense | Self-hosted Algolia alternative | Medium |
| Elasticsearch | Very large scale, complex queries | High |

### PostgreSQL Native FTS
```sql
-- Add search vector column
ALTER TABLE documents ADD COLUMN search_vector tsvector;

-- Auto-update via trigger
CREATE TRIGGER documents_search_update
  BEFORE INSERT OR UPDATE ON documents
  FOR EACH ROW EXECUTE FUNCTION
  tsvector_update_trigger(search_vector, 'pg_catalog.english', 'title', 'body');

-- GIN index for performance
CREATE INDEX idx_documents_search ON documents USING GIN(search_vector);

-- Query
SELECT * FROM documents
WHERE search_vector @@ plainto_tsquery('english', $query)
ORDER BY ts_rank(search_vector, plainto_tsquery('english', $query)) DESC
LIMIT 20;
```

### Risks to Flag
- R-DB-05: GIN index required; full-table search scan is unusable at scale
- R-PERF-02: Cache frequent search results (autocomplete, top queries)
- R-AUTH-04: Search results must be filtered by user/org ownership

---

## 9. AI / LLM Integration

### Streaming Pattern (Recommended)
```typescript
// Backend: stream from LLM to client via SSE
const stream = await anthropic.messages.stream({
  model: 'claude-sonnet-4-20250514',
  messages: [...],
  max_tokens: 1024
});

for await (const chunk of stream) {
  res.write(`data: ${JSON.stringify(chunk)}\n\n`);
}
res.end();
```

### Usage Tracking Schema
```sql
CREATE TABLE ai_usage (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  org_id UUID REFERENCES organisations(id),
  model VARCHAR NOT NULL,
  prompt_tokens INTEGER NOT NULL,
  completion_tokens INTEGER NOT NULL,
  cost_usd DECIMAL(10,6),
  feature VARCHAR,          -- 'summarise', 'chat', 'generate'
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Rate Limiting Strategy
- Per-user per-day token budget
- Per-org per-month spend cap
- Hard cutoff with graceful error message

### Risks to Flag
- R-API-03: LLM endpoints MUST be rate limited (cost control + abuse)
- R-SEC-02: Sanitise user input before embedding in prompts (prompt injection)
- R-PERF-04: Long-running LLM calls should be async jobs, not blocking requests
- R-3P-01: Implement fallback or graceful degradation for API outages

---

## 10. Admin Panel & Audit Logs

### Audit Log Schema
```sql
CREATE TABLE audit_log (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  actor_id UUID REFERENCES users(id),
  actor_email VARCHAR,        -- denormalised for historical accuracy
  action VARCHAR NOT NULL,    -- 'user.delete', 'billing.refund', 'role.change'
  target_type VARCHAR,        -- 'user', 'org', 'subscription'
  target_id UUID,
  metadata JSONB,             -- before/after state, IP address, etc.
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_audit_log_actor ON audit_log(actor_id, created_at DESC);
CREATE INDEX idx_audit_log_target ON audit_log(target_type, target_id, created_at DESC);
```

### Admin Route Protection Pattern
```typescript
// Separate router with stricter middleware
const adminRouter = express.Router();
adminRouter.use(requireAuth);
adminRouter.use(requireRole('admin'));
adminRouter.use(logAdminAction);  // audit every admin request
```

### Impersonation Pattern (if needed)
```typescript
// Store original admin ID in session during impersonation
session.impersonating_as = targetUserId;
session.impersonated_by = adminUserId;

// Audit log every action taken while impersonating
```

### Risks to Flag
- R-AUTH-03: Admin routes must be protected at router level, not per-handler
- R-OBS-02: Every sensitive admin action must write to audit_log
- R-AUTH-06: Admin API endpoints are high-value BOLA targets — verify ownership everywhere
- R-DEPLOY-03: Feature-flag new admin capabilities for staged rollout

---

*Reference: Load this file in Phase 3 and Phase 5 when the requested feature matches a blueprint above. Use as a starting point; always adapt to the actual project stack and patterns.*

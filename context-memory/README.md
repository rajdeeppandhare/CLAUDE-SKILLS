# 🧠 Context Memory — Claude Skill

> A portable conversation memory skill for Claude that snapshots your entire session into a downloadable file — so you can resume exactly where you left off in any new chat, or even on a different model.

---

## ✨ What It Does

Hit your token limit? Switching models? Want to save your progress? Context Memory builds a rich **Memory Map** of your conversation that captures everything needed to resume seamlessly:

- 🗺️ **Maps** your entire session — goals, decisions, code, issues, dead ends
- 📥 **Exports** a clean Markdown file you can download anytime
- 🔄 **Resumes** instantly — upload the file to any new chat and pick up mid-sentence
- 🤖 **Cross-model** — works with any Claude instance, or any AI that can read Markdown
- ⚡ **Proactive** — Claude can offer a checkpoint automatically when sessions get long

---

## 📋 What Gets Captured

| Section | What's Inside |
|---|---|
| 🎯 Session Goal | What you were trying to build or solve |
| 📍 Current Status | Exact state + next planned step |
| ✅ Accomplishments | Every completed step, in order |
| 🐛 Issues & Resolutions | Problems found, fixed, or still open |
| 💻 Code & Artifacts | All files produced, with full content |
| 🔑 Key Decisions | Why you chose X over Y |
| ⚠️ Dead Ends | What failed — so you don't repeat it |
| 📦 Environment | OS, language, deps, env vars, paths |
| 💬 Chat Highlights | The exchanges that actually mattered |
| 🚀 Resume Instructions | Exact prompt to hand to the next Claude |

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `context-memory.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `context-memory/` folder
3. Rename to `context-memory.skill`
4. Install via Claude.ai **Settings → Skills**

---

## 🚀 How to Use

### Saving a Memory Map
Once installed, just ask naturally:

```
"Save my progress"
"Export this conversation"
"I'm running out of tokens — checkpoint this"
"Create a memory map"
"I want to continue this in a new chat"
```

Claude will generate and export a `memory-map-*.md` file you can download.

### Resuming in a New Chat
1. Start a fresh chat
2. Upload your `memory-map-*.md` file
3. Say:

```
"Resume from my memory map"
```

Claude will read the file, confirm the current state, and pick up exactly where you left off — no re-explaining needed.

---

## 📊 Example Output

```markdown
# 🧠 Conversation Memory Map
**Exported:** 2026-05-31
**Model:** Claude Sonnet 4.6

## 🎯 Session Goal
Build a JWT authentication system for a Node.js/Express API with
refresh token rotation and Redis session storage.

## 📍 Current Status
**Where we are:** Auth middleware is working; refresh token endpoint returns 200
**Next planned step:** Implement token blacklisting on logout
**Confidence level:** ~65% complete

## ✅ What Was Accomplished
1. Set up Express server with basic routing
2. Implemented /login endpoint with bcrypt password check
3. JWT access token generation working (15min expiry)
4. Refresh token stored in Redis with 7-day TTL
5. Auth middleware verified on protected routes

## ⚠️ Dead Ends & Things That Failed
- Tried jsonwebtoken v9 → broke with ESM imports, downgraded to v8.5.1
- Redis connection with `ioredis` default config failed on WSL2 →
  needed explicit host: '127.0.0.1' instead of 'localhost'
...
```

---

## 🔄 Resume Mode

When you upload a Memory Map to a new chat, Claude automatically switches into **Resume Mode**:

```
## 🔄 Session Resumed

Goal: Build JWT auth system for Node.js/Express API
Picking up at: Implement token blacklisting on logout
Tracking these open issues:
  - Redis on WSL2 needs explicit 127.0.0.1 host ⚠️

Stack: Node.js 20, Express 4, Redis via ioredis, WSL2

Is anything different from when this was saved?
Otherwise I'll jump straight into the blacklisting logic now.
```

---

## 💡 Proactive Checkpoints

You don't have to ask — Claude will offer automatically when:

- The conversation gets long and complex
- A major milestone is reached
- You mention running low on tokens
- You say you're stepping away and coming back later

---

## 📁 Skill Structure

```
context-memory/
├── SKILL.md                      # Core skill logic & memory map schema
├── README.md                     # This file
├── context-memory.skill          # Installable skill package
└── references/
    ├── memory-map-schema.md      # Detailed schema guide for every section
    └── resume-mode-patterns.md   # Resume mode behaviour & edge cases
```

---

## 🧠 Built With

- [Claude Skills](https://claude.ai) — Anthropic's skill system for extending Claude
- Portable Markdown format — readable by any AI or human

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built as a portfolio project demonstrating Claude skill development — solving the real problem of lost context across long AI sessions.*

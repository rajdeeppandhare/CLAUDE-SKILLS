---
name: context-memory
description: >
  Activate this skill to build and export a portable conversation memory map — a structured snapshot of the current chat that can be downloaded as a file and uploaded to a new session (or a different model) to resume exactly where things left off. Use this whenever the user says things like: "save my progress", "export this conversation", "I'm running out of tokens", "I want to continue this in a new chat", "create a memory snapshot", "save context", "make a resumable checkpoint", "export memory", "context is getting long", or when Claude notices the conversation is approaching its limit. Also trigger proactively when a complex debugging, coding, planning, or research session is underway and the user might benefit from a checkpoint. The exported artifact is a self-contained Markdown file that any Claude instance (or compatible AI) can read to resume the session with full context.
---

# Context Memory Skill

## Purpose

Build a rich, portable snapshot of the current conversation that can be:
1. Downloaded as a `.md` file (the **Memory Map**)
2. Uploaded to a new chat to resume without re-explaining anything

---

## When to Activate

- User explicitly asks to save/export/checkpoint the conversation
- User mentions running out of tokens or context space
- User wants to continue in a new chat or switch models
- Claude detects the conversation is long and complex (proactive offer)
- A major milestone or issue resolution just occurred

---

## Step 1: Build the Memory Map

Read the **entire conversation** carefully, then construct the map using the schema below. Be thorough — the goal is that someone reading only this file could pick up exactly where things left off.

### Memory Map Schema

```
# 🧠 Conversation Memory Map
**Exported:** [timestamp / session date]
**Model:** [model name if known]
**Skill Version:** 1.0

---

## 🎯 Session Goal
[1–3 sentences: what the user was trying to accomplish overall]

---

## 👤 User Context
[Who the user is, their expertise level, relevant background shared in chat]
- Role / background: ...
- Tech stack / environment: ...
- Constraints or preferences mentioned: ...

---

## 📍 Current Status
**Where we are:** [One sentence — what is the exact state right now?]
**Next planned step:** [What was about to happen when the session ended?]
**Confidence level:** [How close to done? e.g. "~70% complete", "stuck on X", "ready to test"]

---

## ✅ What Was Accomplished
[Numbered list of completed steps, decisions made, things that worked]
1. ...
2. ...

---

## 🐛 Issues & Resolutions
[Each significant problem encountered, and what fixed it — or if it's unresolved]

| Issue | Status | Resolution / Notes |
|-------|--------|-------------------|
| ... | ✅ Resolved | ... |
| ... | ❌ Unresolved | ... |
| ... | ⚠️ Workaround | ... |

---

## 💻 Code / Artifacts Produced
[List every file created, modified, or discussed. Include the full content or a summary if too long]

### [filename]
```language
[full code — or if >100 lines, a summary + the most critical sections]
```

---

## 🔑 Key Decisions
[Important choices made during the session, and why]
- Decided to use X instead of Y because...
- Chose approach A over B because...

---

## ⚠️ Dead Ends & Things That Failed
[What was tried and didn't work — critical to avoid re-doing these]
- Tried X → failed because Y
- Approach Z was abandoned because...

---

## 📦 Environment & Setup
[Everything needed to reproduce the working environment]
- OS / platform: ...
- Language / runtime version: ...
- Dependencies installed: ...
- Config / env vars needed: ...
- File paths or directory structure: ...

---

## 💬 Conversation Highlights
[3–7 verbatim or near-verbatim exchanges that carry the most signal — decisions, clarifications, key errors]

**User:** ...
**Claude:** ...

---

## 🚀 Resume Instructions
[Exact prompt the user (or the file itself) should give to the next Claude instance]

> "I'm resuming a session. Here's the memory map: [this file was uploaded]. 
> Please read it fully, confirm you understand the current state, 
> then continue from: [NEXT PLANNED STEP]."

---

## 📎 Raw Notes
[Anything else worth preserving — links, references, partial ideas, things to revisit]
```

---

## Step 2: Export as Artifact

After building the map in your head, produce it as a **downloadable Markdown file** via `create_file` saved to `/mnt/user-data/outputs/memory-map-[short-topic].md`, then call `present_files` so the user can download it.

**Naming convention:** `memory-map-[2-4 word topic slug].md`
Examples: `memory-map-react-auth-bug.md`, `memory-map-api-refactor.md`

---

## Step 3: Give the User a Resume Prompt

After presenting the file, give the user this message (adapted to their situation):

> **To resume in a new chat:** Upload the `memory-map-*.md` file and say:
> *"Please read this memory map and resume from [next step]."*

---

## When a Memory Map is UPLOADED (Resume Mode)

If the user uploads a `.md` file that starts with `# 🧠 Conversation Memory Map`, switch into **Resume Mode**:

1. **Read the entire file carefully**
2. Respond with a structured acknowledgment:
   - Confirm the session goal
   - Confirm current status + next planned step  
   - List any unresolved issues you'll keep in mind
   - Ask if anything has changed since the snapshot
3. Then immediately continue the work — don't wait for the user to re-explain

### Resume Acknowledgment Template

```
## 🔄 Session Resumed

**Goal:** [session goal from map]
**Picking up at:** [next planned step]
**Unresolved issues I'm tracking:** [list]
**Environment:** [key env details]

Is anything different from when this was saved? Otherwise I'll continue from [next step] now.
```

---

## Quality Guidelines

- **Be generous with code** — include full file contents if under ~150 lines; for longer files, include the most-changed sections + a summary
- **Dead ends are gold** — document what failed as carefully as what worked
- **Environment details matter** — missing a version number or env var is enough to derail a resume
- **Write for a stranger** — imagine the reader has never seen this conversation
- **Conversation highlights** — pick exchanges that carried real decisions or revealed important constraints, not just pleasantries

---

## Proactive Checkpoint Offer

If Claude notices the conversation is getting long and complex (major coding project, multi-step debugging, long planning session), it may proactively offer:

> "This session is getting substantial — want me to generate a Memory Map checkpoint you can save and resume from later?"

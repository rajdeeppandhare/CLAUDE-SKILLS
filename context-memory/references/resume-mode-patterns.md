# 🔄 Resume Mode Patterns

This file defines how Claude should behave when a Memory Map is uploaded to a new session. It covers detection, acknowledgment templates, and handling edge cases.

---

## Detection: Is This a Memory Map?

A file is a Memory Map if it:
1. Is a `.md` file uploaded by the user
2. Starts with the header `# 🧠 Conversation Memory Map`
3. Contains at least the sections: Session Goal, Current Status, Next Planned Step

If all three are true → switch to **Resume Mode** immediately.

---

## Resume Mode: Step-by-Step

### Step 1: Read Everything
Read the entire file before responding. Do not skim. Pay special attention to:
- **Unresolved Issues** — these are active blockers
- **Dead Ends** — do not suggest these approaches again
- **Environment** — match the user's exact stack
- **Next Planned Step** — this is where you start

### Step 2: Acknowledge
Respond with the Resume Acknowledgment (see template below). Keep it concise — the user wants to get back to work, not read a recap of their own file.

### Step 3: Clarify Changes
Always ask: *"Is anything different from when this was saved?"*
Common changes:
- They fixed something on their own since the export
- They changed their mind about an approach
- New constraints appeared
- They're on a different machine/environment

### Step 4: Resume
After the user confirms (or makes corrections), continue the work from the Next Planned Step. Do not ask for re-explanation of anything already in the map.

---

## Resume Acknowledgment Template

```
## 🔄 Session Resumed

**Goal:** [session goal, one sentence]
**Picking up at:** [next planned step verbatim from map]
**Tracking these open issues:**
- [issue 1 — status]
- [issue 2 — status]

**Stack:** [key env details — language, framework, OS if relevant]

Is anything different from when this was saved? Otherwise I'll jump straight into [next step] now.
```

---

## Edge Cases

### Map is incomplete or truncated
If the map is missing key sections (e.g. no code, no environment), note what's missing and ask the user to fill in the gaps before proceeding. Don't guess.

### Map describes work Claude didn't do
Treat it the same. The map is the source of truth regardless of which Claude instance produced it.

### Multiple maps uploaded
If the user uploads more than one map, ask which session to resume, or if they should be merged. Do not assume.

### Map is from a different model (e.g. GPT → Claude)
Still valid. Read and resume normally. Note any tool or format differences if relevant.

### Map is very old
If the export date suggests significant time has passed (weeks/months), flag that dependencies or APIs may have changed and suggest a quick environment check.

### User says "update the map"
If the user asks to refresh or update the memory map mid-session:
1. Re-read the original map
2. Add new accomplishments, resolved issues, new dead ends
3. Update Current Status and Next Planned Step
4. Export the updated version

---

## What NOT To Do in Resume Mode

- ❌ Don't ask the user to re-explain the project
- ❌ Don't suggest approaches listed in Dead Ends
- ❌ Don't ignore unresolved issues — acknowledge them
- ❌ Don't assume the environment is the same as your defaults — use what's in the map
- ❌ Don't write a long recap — the user already knows their own project
- ❌ Don't start working before asking if anything has changed

---

## Proactive Checkpoint Triggers

Claude should offer to generate a Memory Map when:

| Trigger | Example |
|---------|---------|
| Long session | Conversation has 20+ exchanges |
| Complex work | Multi-file coding, architecture design, debugging |
| User mentions running out of tokens | "we're getting close to the limit" |
| Major milestone completed | "ok that's finally working" |
| User about to switch contexts | "I'll come back to this later" |
| Explicit request | "save my progress", "export this", "create a checkpoint" |

**Offer message:**
> "This session is getting substantial — want me to generate a Memory Map checkpoint you can save and resume from later?"

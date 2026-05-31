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

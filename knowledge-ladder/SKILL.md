---
name: knowledge-ladder
description: A progressive learning engine that takes any topic and builds a structured path from zero understanding to expert mastery — across 7 levels (Five-Year-Old → Researcher). Instead of giving one explanation at one unknown level, it detects where the user currently is, delivers the right level, offers to move up or down, identifies prerequisite gaps, maps the full learning graph, uses analogies from the user's world, catches misconceptions, and ends with a Mastery Check (Explain → Compare → Apply → Critique → Create). Use this skill whenever the user asks to "learn X", "explain X", "teach me X", "I don't understand X", "explain X like I'm 5", "explain X simply", "explain X in depth", "I want to understand X from scratch", "break down X for a beginner", "how does X work", "I'm confused about X", or any request that involves understanding a concept, technology, theory, domain, or subject from any level of familiarity. Always use this skill when the user wants to understand something — even casual phrasing like "what even is X?" or "ok but how does X actually work?" qualifies.
---

# Knowledge Ladder

Turns any topic into a structured climb from zero to mastery — one level at a time, always at the right altitude for the user.

---

## The problem this skill exists to solve

Normal explanations pick one level and hope it lands. Too simple → bored. Too complex → lost. No path forward either way. This skill never gives a static answer — it gives a **level-aware explanation** with a clear next rung on the ladder.

---

## The 7 Levels

| Level | Label | Audience | What changes |
|-------|-------|----------|-------------|
| 0 | Five-Year-Old | No prior knowledge | Pure analogy, zero jargon, story-form |
| 1 | Beginner | Curious adult | Named concepts, no assumed vocab |
| 2 | Intermediate | Student / self-learner | Structure, relationships, light mechanics |
| 3 | Advanced | Practitioner | Math/code where useful, real tradeoffs |
| 4 | Professional | Working expert | Nuance, edge cases, production concerns |
| 5 | Researcher | Domain specialist | Open problems, frontier debates, citations |
| 6 | Expert | Thought leader | Where knowledge ends, what's unknowable |

---

## Process

### Step 1 — Level Detection
Before explaining, determine the user's current level. Use signals from their message:
- Vocabulary they used (or avoided)
- How they framed the question
- Any prior context in conversation

If ambiguous, **ask one short question** ("How familiar are you with X — total beginner, or have you seen some of it before?") or default to **Level 1** and offer to adjust.

Never assume expert level unless the user demonstrates it.

### Step 2 — Prerequisite Scan
Before explaining, check: does this topic require foundation concepts the user likely lacks? If so, **name them briefly** and offer to explain them first or embed micro-explanations inline. See `references/prerequisites.md` for common prerequisite chains.

### Step 3 — Deliver the Explanation
Use the level-specific format below. Each level has its own output structure — do not collapse them.

For levels 0–2: lean on analogy. Choose the analogy domain from user cues (sports, cooking, gaming, finance, etc.) or ask. See `references/analogy-engine.md`.

For levels 3–6: include structure (formulas, diagrams, code, tradeoffs) as appropriate.

### Step 4 — Navigation Footer
Every explanation ends with a footer offering:
```
📍 You're at Level [N]: [Label]
⬆️  Go deeper → [one-line preview of Level N+1]
⬇️  Simpler → [one-line preview of Level N-1]
🗺️  Prerequisites: [list if any gaps detected]
🧪  Ready to test your understanding? → say "quiz me"
```

### Step 5 — Interactive Modes (on request)
Activate these when the user asks:

- **"Quiz me"** → Mastery Check (see below)
- **"Explain it with [X analogy]"** → Analogy Engine
- **"What do I need to know first?"** → Learning Graph
- **"Is it true that..."** → Misconception Detector
- **"Give me the full path"** → Curriculum Builder
- **"Explain it back and check me"** → Reverse Teaching

---

## Level Output Formats

### Level 0 — Five-Year-Old
- Story or short scene. No technical terms whatsoever.
- If a term must appear, immediately replace it with a concrete everyday object.
- Max 150 words. Warm, encouraging tone.

### Level 1 — Beginner
- Named concepts introduced one at a time.
- Analogy first, then the real term.
- Simple input→output framing where possible.
- Bullet points OK. No formulas.

### Level 2 — Intermediate
- Show structure: layers, components, flow.
- Use simple diagrams (ASCII or arrow notation).
- Introduce mechanism ("how it actually works") without full math.
- Mention what can go wrong at a high level.

### Level 3 — Advanced
- Add math/code where it clarifies (not just to look technical).
- Name the key formula or algorithm and explain each term.
- Real tradeoffs: when this approach wins, when it fails.
- Reference real-world systems that use this.

### Level 4 — Professional
- Production concerns: scale, edge cases, failure modes.
- Competing approaches and when to choose each.
- Nuance: what the textbook version gets wrong in practice.
- Benchmarks, costs, tooling choices.

### Level 5 — Researcher
- Current state of the literature.
- Key papers, seminal results, recent developments.
- Open questions the field hasn't solved.
- Where consensus exists vs. where it's contested.

### Level 6 — Expert
- The edge of the map: what we don't know and why.
- Fundamental limits (theoretical or practical).
- Framing the right research questions.
- Invite the user's own perspective — at this level, dialogue matters more than lecture.

---

## Mastery Check ("quiz me")

After any explanation, if the user says "quiz me," "test me," or "check my understanding," run the 5-stage mastery check:

```
Stage 1 — EXPLAIN    "Explain [concept] in your own words."
Stage 2 — COMPARE    "What's the difference between [concept] and [related concept]?"
Stage 3 — APPLY      "How would you use [concept] to solve [scenario]?"
Stage 4 — CRITIQUE   "What's wrong with this approach: [common mistake]?"
Stage 5 — CREATE     "Design a [small system/example] that uses [concept]."
```

After each response, give feedback:
- ✅ What they got right (name the principle)
- ⚠️ What was partially right and why
- ❌ What was missing and a micro-correction

After all 5 stages, output a Mastery Report:
```
📊 Mastery Report — [Topic]
Strong: [areas]
Developing: [areas]
Suggested next: [one specific action]
Overall: [X/5 stages cleared]
```

---

## Misconception Detector

When the user states something as fact that contains a common misconception, surface it clearly but non-condescendingly:

```
⚠️ Common misconception spotted

What people often think:
[restate their belief]

What's actually true:
[correction]

Why the misconception exists:
[brief explanation — this builds trust]
```

See `references/misconceptions.md` for a library of common misconceptions by domain.

---

## Curriculum Builder ("give me the full path")

When the user wants the full learning journey for a topic, output:

```
📚 Learning Path: [Topic]

Estimated time: [X hours]
Difficulty: [N/10]
Prerequisites: [list]

Stage 1 — Foundation ([X hrs])
  └─ [Subtopic A]
  └─ [Subtopic B]

Stage 2 — Core ([X hrs])
  └─ [Subtopic C]
  └─ [Subtopic D]

Stage 3 — Advanced ([X hrs])
  └─ [Subtopic E]

Stage 4 — Mastery ([X hrs])
  └─ Projects / real-world application
  └─ Engaging with primary sources

Recommended resources: [2-3 specific resources, not generic]
```

---

## Failure modes to avoid

- **One-size explanation** — picking a level and never checking if it landed. Always include the navigation footer.
- **Jargon smuggling at low levels** — introducing technical terms at Level 0-1 without immediately grounding them in analogy.
- **Skipping the footer** — the level indicator and navigation options are not optional. They are what make this a ladder, not a single rung.
- **Mastery check without feedback** — never just grade. Always name what principle the user demonstrated or missed.
- **Curriculum without time estimates** — vague paths ("learn linear algebra first") are useless. Always attach rough time and difficulty.

---

## Reference files

- [`references/analogy-engine.md`](references/analogy-engine.md) — Domain-specific analogy libraries (gaming, sports, cooking, finance, cars, cricket, etc.)
- [`references/prerequisites.md`](references/prerequisites.md) — Common prerequisite chains by domain (AI/ML, finance, physics, CS, etc.)
- [`references/misconceptions.md`](references/misconceptions.md) — Library of common misconceptions by subject area

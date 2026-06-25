# knowledge-ladder

> Turn any topic into a structured climb from zero to mastery — one level at a time, always at the right altitude.

---

## What it does

Most explanations pick one level and hope it lands. Too simple → the user is bored. Too complex → the user is lost. Neither gives a path forward.

Knowledge Ladder detects where the user currently is, delivers an explanation at exactly that level, and always provides a clear next rung — up or down — so the user is never stuck.

---

## The 7 Levels

| Level | Label | Who it's for |
|-------|-------|-------------|
| 0 | Five-Year-Old | Pure analogy, zero jargon |
| 1 | Beginner | Named concepts, everyday language |
| 2 | Intermediate | Structure, relationships, light mechanics |
| 3 | Advanced | Math/code, real tradeoffs |
| 4 | Professional | Production concerns, edge cases |
| 5 | Researcher | Literature, open problems |
| 6 | Expert | The edge of what's known |

---

## When it triggers

Claude uses this skill whenever you ask to learn, understand, or explain anything:

- *"Explain neural networks"*
- *"Teach me quantum computing"*
- *"I don't understand gradient descent"*
- *"Explain it like I'm 5"*
- *"How does X actually work?"*
- *"Break this down for a beginner"*
- *"What even is X?"*

---

## Interactive Modes

| You say | What happens |
|---------|-------------|
| `"quiz me"` | 5-stage Mastery Check (Explain → Compare → Apply → Critique → Create) |
| `"give me the full path"` | Curriculum Builder with time estimates and prerequisites |
| `"explain it with a [cooking / gaming / sports] analogy"` | Analogy Engine |
| `"what do I need to know first?"` | Learning Graph with prerequisite chains |
| `"is it true that..."` | Misconception Detector |
| `"explain it back to me and check"` | Reverse Teaching |

---

## Every explanation ends with

```
📍 You're at Level N: [Label]
⬆️  Go deeper → [preview of next level]
⬇️  Simpler → [preview of level below]
🗺️  Prerequisites: [any gaps detected]
🧪  Ready to test understanding? → say "quiz me"
```

---

## Files

```
knowledge-ladder/
├── SKILL.md                          # Core skill — levels, process, output formats, modes
└── references/
    ├── analogy-engine.md             # Domain analogy libraries (gaming, cooking, sports, finance, etc.)
    ├── prerequisites.md              # Prerequisite chains by domain (AI/ML, physics, finance, CS, etc.)
    └── misconceptions.md             # Common wrong beliefs by subject area with corrections
```

---

## Install

Drop the `.skill` file into your Claude skills folder, or install via the Claude skills UI.

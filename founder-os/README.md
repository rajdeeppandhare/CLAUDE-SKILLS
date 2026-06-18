# Founder OS — Skill Package

A persistent cofounder layer for Claude. Turns Claude from a one-shot Q&A tool into something that actually holds context across your startup's decisions, risks, and roadmap — without losing the thread every session.

---

## What it does

Most AI startup advice is cheerleading. Founder OS is built around the opposite assumption: the highest-value thing a cofounder can do is find the holes in your plan *before* a customer or investor does.

**Eight operating phases:**

| Phase | What happens |
|---|---|
| 1 — Load State | Reads your Founder State Document before responding to anything |
| 2 — Challenge Mode | Interrogates new ideas like a skeptical investor, not a hype machine |
| 3 — Validation | Assumption hunting, ICP definition, customer discovery scripts |
| 4 — Market Intel | Competitive research synthesis, bottom-up market sizing |
| 5 — Planning | PRD generation, roadmap staging, prioritization scoring |
| 6 — Execution | Weekly planning, progress tracking, meeting transcript processing |
| 7 — Risk Audit | Contradiction detection between docs, risk register, burn rate |
| 8 — Handoff | Full updated Founder State Document to save and carry forward |

---

## How memory works (important)

Claude has no persistent memory between conversations by default. Founder OS solves this with a **Founder State Document** — a plain markdown file you own and control.

**The loop:**
1. You save the Founder State Document after each session
2. You paste or upload it at the start of the next session
3. Claude reads it fully before responding — decisions, risks, roadmap and all
4. At the end, you get the updated document back

The document is human-readable markdown, not a database export. You can open it, edit it, or read it yourself without any tooling.

Schema and full format → `references/founder-state-schema.md`

---

## File structure

```
founder-os/
├── founder-os.skill                              ← Install this
└── references/
    ├── founder-state-schema.md                   ← State doc format
    ├── challenge-mode-playbook.md                ← Skeptical investor Q&A bank
    ├── planning-templates.md                     ← PRD, roadmap, prioritization
    ├── market-and-competitive-intelligence-playbook.md  ← Research source strategy
    └── decision-and-risk-log-patterns.md         ← Decision log & risk register formats
```

---

## Installation

1. Download and unzip `founder-os.zip`
2. In Claude, go to **Skills** (or your skill manager)
3. Install `founder-os.skill` — the references folder installs alongside it automatically
4. Start a conversation and paste or upload your Founder State Document if you have one

---

## Trigger phrases

Founder OS activates on:

- "Help me think through this idea"
- "Should I build this?"
- "Poke holes in my plan"
- "Update my roadmap"
- "Process these meeting notes"
- "What are the risks here?"
- "Here's my founder state doc"
- Any mention of PRD, ICP, runway, pivot, or competitors **for your own venture**
- "Founder mode" / "cofounder" / "startup OS"

---

## Quickstart (first session, no state doc yet)

Just tell Claude what you're building. Something like:

> *"I'm building [product]. The target user is [who]. I'm at the [idea/MVP/beta] stage. I want to [think through the idea / validate the market / build a roadmap]."*

Claude will capture the basics and start building your Founder State Document from scratch. You don't need to fill in a form — partial state is fine and it fills in over time.

---

## Challenge Mode

The most important module. Before any new idea or feature gets built out, Claude runs deliberate skepticism:

- What's the evidence beyond intuition?
- Who specifically has confirmed they'd pay/switch/adopt?
- What stops a well-resourced competitor from copying this?
- What's the riskiest unverified assumption?

This is constructively skeptical, not hostile. The goal is a founder who's pressure-tested their own thinking — not one who feels attacked.

After Challenge Mode, you get a structured verdict:
```yaml
Recommendation: pursue / pursue with changes / avoid
Strongest part of the idea: [...]
Riskiest unverified assumption: [...]
Suggested next step: [smallest thing that tests the riskiest assumption]
```

---

## Reference files at a glance

| File | Contents |
|---|---|
| `founder-state-schema.md` | Full Founder State Document schema, how to read and update it |
| `challenge-mode-playbook.md` | Full investor question bank, tone calibration, output shape |
| `planning-templates.md` | PRD template, roadmap stage format, RICE-adjacent prioritization scoring |
| `market-and-competitive-intelligence-playbook.md` | Source strategy by question type, competitive tracking template, market synthesis format, anti-patterns |
| `decision-and-risk-log-patterns.md` | Decision log entry format, risk register format, contradiction detector patterns, meeting processor output format |

---

## Design principles

- **Skepticism over cheerleading** — Challenge Mode runs before building, not after
- **Earned trust** — if you have good evidence, Claude acknowledges it instead of demanding more proof for its own sake
- **Explicit reasoning** — decisions log *why*, not just *what*, so you can tell a good old decision from a bad one later
- **No fabrication** — financials, market sizes, and competitor details only come from what you've provided or from actual research, never invented
- **Human-readable state** — the Founder State Document is plain markdown you can open, read, and edit without any tooling

---

## Built by

Designed as a `.skill` package for Claude. References are loaded progressively — only the relevant module gets read per phase, so a weekly planning update doesn't pull the full competitive intelligence playbook into context unnecessarily.

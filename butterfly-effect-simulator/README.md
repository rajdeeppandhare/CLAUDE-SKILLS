# 🦋 Butterfly Effect Simulator

**A pre-decision consequence mapping skill for Claude that branches a decision into forking causal trees — not linear chains — tags every node with confidence, reversibility, and blast radius, and runs the counterfactual path alongside the real one.**

Most consequence-mapping is a single cherry-picked story: A→B→C→D. That's a narrative, not an analysis — it picks one path through a much wider set of possibilities and quietly discards the rest. Butterfly Effect Simulator forces the fan-out: every decision branches into multiple plausible downstream effects, simultaneously, with confidence that decays the deeper the chain goes.

The natural **pre-decision** counterpart to **Decision Audit** in this portfolio: Butterfly Effect maps what's foreseeable before you act, Decision Audit grades whether your reasoning held up after.

---

## 🎯 The Core Insight

Real consequences fork, they don't chain. "Use a third-party API" doesn't lead to one outcome — it simultaneously affects cost, vendor relationship, time-to-market, and compliance, each branching further on its own:

```
Use OpenAI API
├─→ Higher costs ─→ Lower margins ─→ Harder enterprise sales
├─→ Vendor lock-in ─→ Pricing power shifts to OpenAI at renewal
├─→ Faster time-to-market ─→ Earlier customer feedback ─→ Better PMF signal
└─→ Data leaves your infra ─→ Compliance objections ─→ Longer sales cycles
```

And confidence drops as the chain deepens — a 1-hop mechanical fact and a 4-hop speculative chain should never read with the same certainty.

---

## 🧱 How It Works

### Branching, not chaining
Every node fans out into 1–4 genuinely distinct downstream effects — across cost, relationships, capability, and perception — instead of picking the single most dramatic story.

### Four tags on every node
| Tag | What it captures |
|---|---|
| **Type** | Mechanical (near-certain) → Behavioral (depends on response) → Speculative (compounding assumptions) |
| **Confidence** | Decays with depth — 80-95% at hop 1, often under 40% by hop 3+ |
| **Reversibility** | Easy to undo / costly to undo / irreversible |
| **Blast radius** | Contained / team / company / external |

**Priority rule:** any node tagged irreversible + company/external blast radius gets flagged explicitly, regardless of depth or confidence. A 15%-likely irreversible event deserves more attention than a 90%-likely trivial one.

### The counterfactual path
Every simulation runs the inaction (or next-best alternative) tree side by side with the real decision — turning the output into an actual tradeoff, not just a prediction.

### Feedback loop detection
Scans the whole tree for nodes that loop back into earlier causes:
- **Reinforcing loops** — the effect compounds the original problem (highest-priority finding in the whole system)
- **Self-correcting loops** — the effect naturally dampens the original problem

### Cheapest intervention point
After mapping everything, the skill names the single highest-leverage edge in the tree — the specific, concrete place to add a safeguard before committing, not a vague "be careful."

---

## 📋 Example Output (abridged)

```
Node: Use OpenAI API → Higher per-unit costs
  Type: mechanical | Confidence: 92%
  Reversibility: easy | Blast radius: contained

  └─→ Node: Higher costs → Lower margins (if pricing unchanged)
        Type: behavioral | Confidence: 60%
        Reversibility: costly | Blast radius: company

        └─→ Node: Lower margins → Harder enterprise sales conversations
              Type: speculative | Confidence: 35%
              Reversibility: costly | Blast radius: external

🔄 REINFORCING LOOP DETECTED
Path: Lower margins → Harder fundraising → Less capital → Can't fix cost
      problem → back to Lower margins
Closure depth: 4 hops
Why it matters: This isn't a one-time cost — it's a structural trap that
gets worse without intervention.

CHEAPEST INTERVENTION POINT
Edge: "Use OpenAI API" → "Higher per-unit costs"
Action: Negotiate a volume-pricing cap before signing, not after scale
Why here: Kills the cost-escalation branch at its root — every downstream
effect in this tree depends on costs actually rising.
```

---

## 🗂 Skill Package Contents

```
butterfly-effect-simulator/
├── SKILL.md                              # Seven-phase pipeline (install this)
├── README.md                             # This file
└── references/
    ├── tree-generation-playbook.md       # Branching method, depth guidance, worked example
    ├── tagging-rubric.md                 # Type/confidence/reversibility/blast-radius definitions
    ├── feedback-loop-patterns.md         # Reinforcing vs self-correcting loop detection
    └── output-schema.md                  # Text + JSON output, with direct Decision Audit handoff
```

---

## 🚀 Installation

**Claude.ai Projects (recommended):**
1. Open or create a Project in Claude.ai
2. Go to **Project Settings → Custom Instructions**
3. Paste the full contents of `SKILL.md`
4. Upload all files from `references/` to the Project knowledge base
5. Pairs well in the same Project as **Decision Audit** — the output schema is built to drop straight into a Decision Audit log entry

**One-off use:**
Paste `SKILL.md` directly into any conversation before stating your decision.

---

## 💬 Trigger Phrases

```
"What happens if I [decision]?"
"Map the consequences of switching to X"
"Second-order effects of raising prices"
"What could go wrong if I make this hire?"
"Run the butterfly effect on this decision"
"Compare doing this vs. not doing it"
```

---

## 📐 Design Principles

- **Branch, don't chain** — every node forks into genuinely distinct downstream effects, not a single cherry-picked narrative
- **Confidence decays with depth** — a 4-hop speculative chain is never presented with first-hop certainty
- **Irreversible + high-blast-radius beats raw probability** — a rare catastrophe gets flagged ahead of a common inconvenience
- **The counterfactual is mandatory** — no tree without the inaction/alternative path for comparison
- **Loops over chains** — a feedback loop is usually the most important structural finding in any tree, and gets a dedicated detection phase
- **Every simulation ends in an action**, not just a diagram — the cheapest intervention point is required output

---

## 🔗 Fits the Decision Pipeline

```
Founder OS (ongoing strategic context)
   ↓
Butterfly Effect Simulator  ← map consequences, pick the path, find the cheap fix
   ↓
[decision made + logged]
   ↓
Decision Audit  ← was this a good decision given what was knowable at the time?
```

The output schema's `decision_audit_handoff` block maps directly onto Decision Audit's logging fields (`decision`, `confidence`, `key_assumption`, `decision_type`), so the predicted chain becomes the baseline that gets graded later — no duplicate modeling work between the two skills.

Part of the [CLAUDE-SKILLS](https://github.com/rajdeeppandhare/CLAUDE-SKILLS) portfolio — production-quality skill packages for Claude.ai users.

---

## 📄 License

MIT — use freely, attribution appreciated.

Built for the Claude.ai non-developer community. No API key required, no code to run. Drop the skill into a Project and state your decision.

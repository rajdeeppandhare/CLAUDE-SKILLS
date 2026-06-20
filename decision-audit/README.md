# 🧭 Decision Audit

**A post-decision quality analyzer for Claude that separates "was it a good decision" from "did it work out" — and mines a running decision log for calibration errors and recurring blind spots.**

Most decision journals just track outcomes: good or bad. That's a binary label with no information about *why* it went well or badly — six months later you only know "I'm 60% good at deciding things," which is useless. Decision Audit separates **decision quality** from **outcome luck**, so you stop reinforcing lucky bad decisions and stop abandoning unlucky good ones.

The natural counterpart to **Decision OS** in this portfolio: Decision OS helps you decide. Decision Audit tells you whether your deciding is actually any good.

---

## 🎯 The Core Insight

A good decision can have a bad outcome. A bad decision can have a good outcome. Conflating these is the single biggest trap in any decision-tracking tool.

```
                    GOOD OUTCOME           BAD OUTCOME
                  ┌─────────────────────┬─────────────────────┐
GOOD DECISION     │   ✅ DESERVED WIN    │    🍀 BAD LUCK       │
                  ├─────────────────────┼─────────────────────┤
BAD DECISION      │   🎲 LUCKY BREAK     │   ❌ DESERVED LOSS    │
                  └─────────────────────┴─────────────────────┘
```

The two off-diagonal quadrants are where the actual learning lives:
- **Bad Luck** → don't abandon a sound process because it failed once
- **Lucky Break** → don't reinforce a flawed process because it happened to work — this is how systematic errors get baked in

---

## 🧱 How It Works

### Logging Mode (at decision time)
Captures the decision *before* the outcome is known — alternatives considered, confidence level, the key assumption being bet on, reversibility, time pressure, and domain. This is the only point where hindsight bias hasn't contaminated the data yet.

### Review Mode (once the outcome is knowable)
Records what happened, classifies whether the outcome was driven by sound/flawed reasoning or by external factors outside your control, and asks: *knowing only what you knew then, would you decide the same way again?* That last question is what isolates decision quality from outcome luck.

### Pattern Analysis (5+ closed decisions)
Mines the log for things no single decision can reveal:

| Scan | What it finds |
|---|---|
| **Calibration** | When you say "8/10 confident," are you right ~80% of the time — or running hot/cold? |
| **Domain Skew** | Are you reliably good at technical calls but reliably bad at hiring calls? That's a delegation signal. |
| **Speed Bias** | Do fast decisions outperform slow ones, or vice versa? Tells you if you're an overthinker or a snap-judger. |
| **Assumption Failure Clustering** | Is there a *specific, repeated* type of wrong assumption — not just "bad at deciding" in general? |
| **Reversibility Mismatch** | Are irreversible decisions getting rushed while reversible ones get over-deliberated? |

---

## 📋 Example Output

```
🔍 REVIEWING: Launch mobile app before web dashboard reaches feature parity

WHAT HAPPENED
Mobile launch shipped on time; 40% of early users churned within 2 weeks
citing missing dashboard features they expected on mobile too.

DECISION QUALITY CHECK (scored before looking at outcome)
✓ Genuine alternatives considered
✗ Confidence calibrated to actual uncertainty
✗ Key assumption reasonable given available info
✓ Deliberation proportional to reversibility

Verdict: BAD decision (passed 2/4 checks)

OUTCOME DRIVER: reasoning_incorrect
The assumption that users would tolerate a mobile-first gap was wrong —
this wasn't bad luck, the reasoning itself didn't hold up.

QUADRANT: ❌ Deserved Loss

Would decide the same way again? No
```

```
NAMED BLIND SPOT

You keep assuming demand/fit without validating it directly before committing.
Evidence: D-009, D-014, D-021

Suggested checkpoint: before any launch decision, get one piece of direct
user validation — not just internal confidence — before locking the date.
```

---

## 🗂 Skill Package Contents

```
decision-audit/
├── SKILL.md                              # Six-phase pipeline (install this)
├── README.md                             # This file
└── references/
    ├── log-schema.md                     # Full field set, captured in two passes
    ├── scoring-methodology.md            # The 2×2, outcome-driver decision tree
    ├── pattern-analysis-playbook.md      # Five scans with methods + interpretation
    └── report-templates.md               # Fill-in-the-blank output formats
```

---

## 🚀 Installation

**Claude.ai Projects (recommended — this skill is built for an ongoing log):**
1. Open or create a Project in Claude.ai
2. Go to **Project Settings → Custom Instructions**
3. Paste the full contents of `SKILL.md`
4. Upload all files from `references/` to the Project knowledge base
5. Keep your running decision log in the Project knowledge base too (markdown or JSON) so it persists and accumulates across sessions

**One-off use:**
Paste `SKILL.md` directly into any conversation, paste your existing log if you have one, and go.

---

## 💬 Trigger Phrases

```
"Log this decision: [decision]"
"I need to review [decision] — here's what happened"
"Run a pattern report on my decision log"
"Am I actually good at deciding, or just lucky?"
"What's my calibration like?"
"Was this actually a good decision or did I just get lucky?"
```

---

## 📐 Design Principles

- **Decision quality and outcome quality are scored separately, in that order** — quality first, outcome revealed second, to prevent outcome bias from leaking backward into the quality verdict
- **Hindsight contamination is actively policed** — if a justification uses facts only known after the fact, the skill interrupts and redirects
- **No pattern claims from thin data** — minimum 5 closed decisions before any cross-decision scan runs, with per-bucket sample-size minimums inside each scan
- **Every finding is traceable to specific decision IDs** — no vibes-based "you seem bad at X"
- **Lucky Breaks are flagged as the highest-priority finding**, not buried — a flawed process that happened to work is the riskiest pattern in the whole system because it's primed to repeat

---

## 🔗 More Skills

Part of the [CLAUDE-SKILLS](https://github.com/rajdeeppandhare/CLAUDE-SKILLS) portfolio — production-quality skill packages for Claude.ai users.

Pairs naturally with:
- **Decision OS** — the pre-decision counterpart; runs a choice through 9 analytical frameworks before you commit
- **Founder OS** — maintains a persistent Founder State Document across sessions; Decision Audit can feed its Decision Log

---

## 📄 License

MIT — use freely, attribution appreciated.

Built for the Claude.ai non-developer community. No API key required, no code to run. Drop the skill into a Project, start logging, and let the pattern report do the rest once you've got a few decisions closed out.

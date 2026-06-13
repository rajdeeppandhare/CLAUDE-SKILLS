---
name: decision-os
description: >
  Decision OS — a full cognitive operating system for high-stakes decisions.
  Use this skill whenever the user is weighing options, facing a fork in the road,
  or stuck on a major life or business choice. Trigger on phrases like "should I",
  "help me decide", "I can't choose between", "what would you do", "pros and cons",
  "I'm torn", "thinking about switching", "is it worth it", "help me think through",
  "big decision", "career change", "quit my job", "start a company", "move cities",
  "accept the offer", "MBA vs", "build vs buy", "hire vs outsource", or any query
  where the user presents two or more paths and wants structured thinking. Also
  trigger proactively when a user describes a situation with clear trade-offs even
  without explicitly asking for help deciding. This skill is dramatically better
  than asking for "pros and cons" — always use it for any decision that involves
  significant time, money, relationships, or identity.
---

# Decision OS 🎯

A full cognitive operating system for decisions. Runs 9 analytical frameworks
in sequence, synthesises them into a final verdict, and surfaces the insight
the user didn't know they needed.

---

## Pipeline Overview

```
User Decision
      ↓
0. Decision Intake & Classification
      ↓
1. Weighted Decision Matrix
      ↓
2. Opportunity Cost Analysis
      ↓
3. Risk Scoring (Upside / Downside / Catastrophe)
      ↓
4. Time & Energy Investment Analysis
      ↓
5. Regret Minimisation Framework
      ↓
6. Identity & Values Alignment Check
      ↓
7. Second-Order Consequences Map
      ↓
8. Pre-Mortem Analysis
      ↓
9. Synthesis & Verdict
```

Read `references/frameworks.md` for deep implementation of each stage.
Read `references/decision-types.md` to calibrate tone and depth by decision category.
Read `references/cognitive-biases.md` to flag relevant biases before delivering the verdict.

---

## Stage 0 — Decision Intake & Classification

Before running any framework, extract and confirm:

```
Decision:        [restate what the user is choosing between]
Options:         [Option A] vs [Option B] (vs [Option C] if applicable)
Decision Type:   [Career | Business | Financial | Relationship | Geographic | Lifestyle | Build vs Buy | Other]
Time Horizon:    [Short <1yr | Medium 1-3yr | Long 3yr+]
Reversibility:   [Easily reversible | Costly to reverse | Irreversible]
Stakes:          [Low | Medium | High | Life-defining]
User's gut lean: [extract from their message if possible, else mark Unknown]
```

Reversibility is the most important classifier. Irreversible + High Stakes →
run all 9 stages. Reversible + Low Stakes → run stages 1, 5, 9 only and note
the user can experiment cheaply.

---

## Stage 1 — Weighted Decision Matrix

Score each option across 5–8 criteria. Criteria are inferred from the decision
type (see `references/decision-types.md`) but always customised to what the
user actually said matters.

Format:
```
Criteria            Weight   Option A   Option B   WScore A   WScore B
─────────────────────────────────────────────────────────────────────
[criterion 1]        0.25      7/10       4/10       1.75       1.00
[criterion 2]        0.20      5/10       9/10       1.00       1.80
...
─────────────────────────────────────────────────────────────────────
TOTAL                1.00                            X.XX       X.XX
```

Scores are 1–10. Weights sum to 1.0. State the winner, but note if the margin
is narrow (< 0.5 points) — the matrix rarely decides alone.

---

## Stage 2 — Opportunity Cost Analysis

For each option, identify what is permanently given up by choosing it.

```
Choosing [Option A] costs you:
  - [What you can't do or have if you pick A]
  - [The version of yourself that would have existed under B]
  - [Time cost: X years / months you cannot recover]

Choosing [Option B] costs you:
  - [What you can't do or have if you pick B]
  - [The version of yourself that would have existed under A]
  - [Time cost: X years / months you cannot recover]
```

Key insight to surface: **The real cost of any choice is not its price —
it is the best alternative forgone.** Name that alternative explicitly.

---

## Stage 3 — Risk Scoring

Three-axis risk analysis for each option:

```
              Option A        Option B
────────────────────────────────────────────────
Upside        [best case]     [best case]
Probability   [X%]            [X%]

Downside      [worst normal]  [worst normal]
Probability   [X%]            [X%]

Catastrophe   [tail risk]     [tail risk]
Probability   [X%]            [X%]
────────────────────────────────────────────────
Risk Profile  [Asymmetric+/-] [Asymmetric+/-]
```

Then state: Is the risk profile **symmetric**, **positively asymmetric**
(big upside, capped downside), or **negatively asymmetric** (capped upside,
open-ended downside)?

Positively asymmetric options with recoverable downsides are almost always
worth taking. Flag this clearly if it applies.

---

## Stage 4 — Time & Energy Investment Analysis

Decisions cost more than money — they cost time, attention, and cognitive load.

```
Option A requires:
  Year 1:   [X hrs/wk | primary focus | side project]
  Year 2-3: [trajectory]
  Peak load period: [when and how intense]
  Other life domains affected: [relationships | health | finances | creativity]

Option B requires:
  [same format]

Energy ROI:
  Option A: [what you get per unit of life energy invested]
  Option B: [what you get per unit of life energy invested]
```

Flag if either option has a **hidden time debt** — a future period of intense
demand that the user may not have accounted for (e.g. MBA year 2 recruiting,
startup fundraise crunch, post-merger integration).

---

## Stage 5 — Regret Minimisation Framework

Attributed to Jeff Bezos. Project to age 80 and look back.

```
Scenario A: You chose [Option A].
  If it worked:  [What does that life look like? What did you build or become?]
  If it failed:  [What happened? How do you feel about having tried?]
  Regret score:  [1-10 — how much do you regret choosing A if it fails?]

Scenario B: You chose [Option B].
  If it worked:  [What does that life look like?]
  If it failed:  [What happened?]
  Regret score:  [1-10]

Scenario C: You never decided (chose the default / status quo).
  5 years later: [What does that life look like?]
  Regret score:  [1-10 — usually the highest]
```

Key question to ask: **"Which failure would you forgive yourself for more easily?"**

---

## Stage 6 — Identity & Values Alignment Check

Decisions that conflict with core identity are hard to sustain even if they're
logically optimal.

```
This decision touches these identity dimensions:
  [ ] Who you are professionally
  [ ] Who you are to your family / relationships
  [ ] Who you are financially (risk-taker vs security-seeker)
  [ ] Your relationship with freedom vs stability
  [ ] Your sense of ambition and legacy

Option A aligns with:    [list values it honours]
Option A conflicts with: [list values it strains]

Option B aligns with:    [list values it honours]
Option B conflicts with: [list values it strains]
```

Flag any **identity debt**: choosing an option that misaligns with core identity
creates psychological friction that compounds over time. It's an invisible tax
on every day of execution.

---

## Stage 7 — Second-Order Consequences Map

Most people think one move ahead. This stage maps to three.

```
Option A:
  1st order: [Immediate, obvious outcome]
    → 2nd order: [What does that enable or prevent?]
      → 3rd order: [Where does that path lead 3-5 yrs out?]

Option B:
  1st order: [Immediate, obvious outcome]
    → 2nd order: [What does that enable or prevent?]
      → 3rd order: [Where does that path lead 3-5 yrs out?]

Non-obvious consequences to flag:
  - [Something the user probably hasn't considered]
  - [A door that opens / closes as a result of this choice]
```

---

## Stage 8 — Pre-Mortem Analysis

Imagine it's 2 years from now and the decision was a disaster. Work backwards.

```
Pre-Mortem: [Option A] Failed

Most likely failure modes:
  1. [Specific, realistic failure scenario]
  2. [Second failure mode]
  3. [Third failure mode]

Root causes in each:
  - [What behaviour or assumption leads to failure 1]

Early warning signs to watch for:
  - [Signal at 3 months that you're on the wrong path]
  - [Signal at 6 months]

Mitigation strategies:
  - [What you could do NOW to reduce failure probability]
```

Repeat for Option B.

**The goal:** Not to scare the user out of a decision, but to make success
more likely by pre-loading awareness of failure modes.

---

## Stage 9 — Synthesis & Verdict

After all stages, synthesise into a single clear output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DECISION OS — SYNTHESIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Decision:         [restate]
Frameworks run:   [list which stages applied]

Framework scoreboard:
  Matrix:         → [A/B/Tie]
  Opportunity cost→ [A/B/Tie]
  Risk profile:   → [A/B/Tie]
  Time ROI:       → [A/B/Tie]
  Regret:         → [A/B/Tie]
  Identity fit:   → [A/B/Tie]
  2nd order:      → [A/B/Tie]
  Pre-mortem:     → [A/B/Tie]

VERDICT:          [Option A / Option B / Needs more info]
Confidence:       [High / Medium / Low]

The insight you might have missed:
  [One thing the analysis surfaced that the user likely didn't frame this way]

Key condition:
  [The one thing that, if it changes, changes the verdict]

If you choose [winner]:
  Do this first:  [single highest-leverage first action]
  Watch for:      [the main early warning sign]

If you choose [other]:
  Make sure:      [critical success condition]
  Biggest risk:   [the one thing most likely to go wrong]

Cognitive biases detected in how this decision was framed:
  [See references/cognitive-biases.md — list 1-2 most relevant]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Reference Files

| File | When to read |
|------|-------------|
| `references/frameworks.md` | Deep implementation details for any stage |
| `references/decision-types.md` | Default criteria sets by decision category |
| `references/cognitive-biases.md` | Biases to detect and flag in Stage 9 |
| `references/worked-examples.md` | Full end-to-end worked examples |

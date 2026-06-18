# Planning Templates

Used in Phase 5 of Founder OS. Contains the PRD template, roadmap-stage format, and the prioritization scoring approach. Load this file when the user asks to write a PRD, build out a roadmap, or rank features/tasks.

---

## Table of Contents

1. [PRD Template](#prd-template)
2. [Roadmap Stage Template](#roadmap-stage-template)
3. [Prioritization Scoring](#prioritization-scoring)
4. [Weekly Planning Format](#weekly-planning-format)
5. [Usage Notes](#usage-notes)

---

## PRD Template

Keep PRDs scoped to what's actually being built next. A PRD that covers everything the product might ever do is a vision doc in disguise — useful for alignment, useless for execution. The test: can an engineer read this and know what to build this sprint?

```markdown
# PRD: [Feature / Product Name]

**Status:** Draft / In Review / Approved
**Author:** [name]
**Last updated:** [date]
**Milestone:** [which roadmap stage this belongs to]

---

## Problem

[One paragraph. What's broken, missing, or painful for who? Be specific about the user and the situation — "users can't do X when Y" is better than "users have trouble with the workflow."]

## Users

**Primary:** [specific ICP — not "small businesses," but the type of person/company who has this problem most acutely]
**Secondary:** [optional — who else benefits, but isn't the design target]

## Goals

[What success looks like. 2–4 items. Make them checkable — "reduce time to first value under 2 minutes" beats "improve onboarding."]

- [ ] [Goal 1]
- [ ] [Goal 2]
- [ ] [Goal 3]

## Non-Goals

[What this explicitly doesn't cover. Saves scope creep arguments later.]

- [Non-goal 1]
- [Non-goal 2]

## Success Metrics

[How you'll know it worked after shipping. Pair each goal with a metric.]

| Goal | Metric | Target | Measurement method |
|---|---|---|---|
| [Goal 1] | [metric] | [target value] | [how to measure] |
| [Goal 2] | [metric] | [target value] | [how to measure] |

## Features / Requirements

### Must Have (this milestone)
- [Requirement 1] — [one line rationale if not obvious]
- [Requirement 2]

### Should Have (this milestone if time allows)
- [Requirement 3]

### Won't Have (this milestone — explicitly deferred)
- [Deferred item 1] — deferred because [reason]

## Design Notes

[Any UX decisions, edge cases, or constraints worth recording. Not a full design spec — link to Figma/mockups if they exist.]

## Technical Notes

[Dependencies, known risks, infrastructure changes needed. Keep it brief — this isn't an engineering design doc, but flag anything that might surprise the implementer.]

## Open Questions

[Things not yet decided. Each should have an owner and a target resolution date, or it'll stay open forever.]

- [ ] [Question] — owner: [name], resolve by: [date]

## Timeline

| Milestone | Target date | Notes |
|---|---|---|
| Design complete | [date] | |
| Dev complete | [date] | |
| QA / testing | [date] | |
| Ship | [date] | |
```

---

## Roadmap Stage Template

Stages give a shared definition of "done" that doesn't depend on someone's gut feeling. The canonical sequence is MVP → Beta → Pilot → Launch → Scale, but adjust if the venture has a different shape (e.g. a marketplace might be Supply → Demand → Liquidity → Launch, or a hardware product might be Prototype → Manufacturing → Distribution → Launch).

```markdown
## Roadmap

**Current stage:** [stage name]

### Stage Definitions

| Stage | What it means | Exit criteria (definition of done) |
|---|---|---|
| MVP | Enough to test the core assumption with real users | [specific, checkable criteria] |
| Beta | Real users, real use, feedback loop, no money yet | [specific, checkable criteria] |
| Pilot | Paying customers (even if small number), product-market fit signal | [specific, checkable criteria] |
| Launch | Public, repeatable acquisition, support system in place | [specific, checkable criteria] |
| Scale | Growth engine working, team scaling to match | [specific, checkable criteria] |

### Current Stage: [Stage Name]

**Exit criteria for this stage:**
- [ ] [Criterion 1 — concrete and checkable]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

**Tasks**

| Task | Owner | Status | Notes |
|---|---|---|---|
| [Task 1] | [owner] | Done / In progress / Blocked | [blocking reason if blocked] |
| [Task 2] | [owner] | | |

**Blocked items**
- [Task] — blocked by [what] — [what would unblock it]

### Upcoming Stages (rough scope only)

**[Next stage]:**
- [High-level thing 1]
- [High-level thing 2]

**[Stage after that]:**
- [High-level thing]
```

---

## Prioritization Scoring

Use this when ranking a list of candidate features or tasks and the right order isn't obvious. The scoring model is RICE-adjacent but reweighted for early-stage companies where retention and revenue signal matter more than raw reach.

### The Four Dimensions

**Impact** — if this works, how much does it move the most important current metric?
```
5 = Core metric moves significantly (retention, activation, revenue)
4 = Meaningfully contributes to core metric
3 = Improves user experience but indirect effect on core metric
2 = Nice to have, small measurable effect
1 = Cosmetic or edge-case only
```

**Effort** — how much engineering + product time does this actually take?
```
5 = < 1 day
4 = 1–3 days
3 = 1 week
2 = 2–4 weeks
1 = > 1 month
```
(Note: effort is scored inversely — less effort = higher score, so it points in the same direction as impact.)

**Risk** — how confident are you this will actually deliver the intended impact?
```
5 = High confidence — validated by customer data or prior analogues
4 = Reasonable confidence — directionally supported by evidence
3 = Uncertain — plausible but untested assumption
2 = Low confidence — mostly intuition
1 = Highly speculative — core assumption unverified
```

**Revenue / Retention Signal** — does this directly help with either paying or keeping customers?
```
3 = Directly tied to acquisition, conversion, or retention
2 = Indirect but meaningful connection
1 = No clear connection to revenue or retention
```

### Scoring Table

```markdown
| Feature / Task | Impact (1–5) | Effort (1–5) | Risk (1–5) | Rev/Ret (1–3) | Total | Priority |
|---|---|---|---|---|---|---|
| [Item 1] | | | | | | |
| [Item 2] | | | | | | |
| [Item 3] | | | | | | |

Total = Impact + Effort + Risk + Rev/Ret (max 18)
```

### How to read the scores

```
15–18: Ship this first — high value, manageable effort, evidence-backed
11–14: Strong candidate — sequence based on dependencies and team bandwidth
7–10:  Do later — either low impact or high effort without strong evidence
< 7:   Deprioritize — revisit when you have more signal or bandwidth
```

### Tiebreakers (when scores are close)

1. **What's blocking other things?** Unblocking work takes priority over equivalent-score independent work.
2. **What tests a key assumption?** If one item produces learning and another produces a feature, prefer learning at early stages.
3. **What do customers specifically ask for?** Named customer requests > inferred customer needs when scores are tied.

### Example

```markdown
| Feature | Impact | Effort | Risk | Rev/Ret | Total | Priority |
|---|---|---|---|---|---|---|
| Fix onboarding drop-off at step 3 | 5 | 4 | 4 | 3 | 16 | 1st |
| Add team collaboration | 4 | 2 | 3 | 2 | 11 | 3rd |
| CSV export | 3 | 4 | 5 | 2 | 14 | 2nd |
| Dark mode | 2 | 3 | 5 | 1 | 11 | 4th |
| AI-generated summaries | 3 | 1 | 2 | 2 | 8 | 5th |
```

---

## Weekly Planning Format

A weekly plan should be short enough to actually follow. The test: if someone reads it on Monday morning, do they know exactly what to do? If not, it's too abstract.

```markdown
## Week of [date]

**This week's focus:** [one sentence — the single most important thing this week advances]

**Carry-over from last week:**
- [Item still in progress] — [current status]

**This week's tasks:**

| Task | Estimated time | Done? |
|---|---|---|
| [Task 1] | [Xh] | [ ] |
| [Task 2] | [Xh] | [ ] |
| [Task 3] | [Xh] | [ ] |

**Blocked / waiting on:**
- [Thing blocked] — waiting on [who/what]

**Key question to answer this week:**
[The one thing you want to know by Friday that you don't know today — ideally, the answer tests a real assumption.]
```

---

## Usage Notes

- **PRD scope discipline**: if writing a PRD results in a "must have" list longer than 8–10 items, push back — that's a launch scope, not a sprint scope. Ask what the smallest version that tests the core assumption would look like.
- **Roadmap vs. backlog**: the roadmap has stages; the backlog has tasks. Don't conflate them. A stage should take weeks to months; individual tasks take hours to days.
- **Scoring is a tool, not a verdict**: the prioritization scores are a forcing function for explicit reasoning, not an algorithm to follow blindly. If the scores produce an ordering that feels obviously wrong, that's useful — it means an assumption in the scoring is wrong. Surface it.
- **Update the Founder State Document**: after generating a PRD or roadmap artifact in a session, fold the key decisions and stage definitions into the state document's Decision Log and Roadmap sections before Phase 8.

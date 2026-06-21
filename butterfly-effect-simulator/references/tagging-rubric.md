# Tagging Rubric

Every node in the tree gets four tags: type, confidence, reversibility, and blast radius. This file defines each.

---

## Tag 1 — Type

Classifies *how* certain the causal mechanism itself is, independent of the confidence percentage.

| Type | Definition | Example |
|---|---|---|
| **Mechanical** | Near-certain, direct, low-assumption. Follows from the decision almost by definition. | "Use a paid API → costs scale with usage" |
| **Behavioral** | Depends on how people, customers, or the market respond. The mechanism is plausible but contingent on a choice or reaction outside the decision-maker's control. | "Higher costs → lower margins" (assumes no price pass-through, no efficiency offset) |
| **Speculative** | Compounding assumptions — usually depth 3+, stacking multiple behavioral or mechanical links. The mechanism requires several other things to also be true. | "Lower margins → harder fundraising → can't hire → can't fix the cost problem" |

**Type tends to correlate with depth but isn't strictly determined by it.** A depth-1 node can be behavioral if it already depends on a response (e.g. "decision becomes public → team morale shifts" is behavioral even at depth 1, because it depends on how the team reacts).

---

## Tag 2 — Confidence

A percentage estimate of how likely this specific causal link is to hold, **given that the parent node occurred.** This is conditional probability, not absolute probability — each node's confidence is relative to its parent already being true.

### Default ranges by type
| Type | Typical confidence range |
|---|---|
| Mechanical | 80–95% |
| Behavioral | 40–75% |
| Speculative | 10–40% |

### Adjustments
- Increase confidence if there's strong precedent (the person or comparable companies have seen this exact pattern before)
- Decrease confidence if the mechanism depends on a specific, narrow condition holding (e.g. "assumes the market doesn't shift in the next 6 months")
- Never assign above 95% — leave room for genuine uncertainty even on mechanical links
- Never assign below 5% — if it's that unlikely, the node probably shouldn't be in the tree at all

**Compounding rule:** A node's *effective* confidence (the likelihood of the whole chain from root to this node holding) is the product of all confidences along the path. State both: the node's own conditional confidence, and optionally the compounded path confidence for nodes 3+ hops deep, since that's often strikingly low and is itself an important finding.

---

## Tag 3 — Reversibility

How hard is it to undo this specific effect once it's happened?

| Tag | Definition | Example |
|---|---|---|
| **Easy to undo** | Can be reversed quickly with minimal cost | Switching API providers if not deeply integrated yet |
| **Costly to undo** | Reversible, but requires meaningful time, money, or relationship repair | Re-platforming after deep integration; rebuilding trust after a layoff |
| **Irreversible** | Cannot be meaningfully undone | A funding round closed at a bad valuation; a customer who churned and won't return; a key hire who already quit elsewhere |

Tag reversibility for the *specific node*, not the root decision — different branches of the same tree often have very different reversibility, which is exactly the point of tagging at the node level.

---

## Tag 4 — Blast Radius

Who or what is affected if this specific node occurs?

| Tag | Definition | Example |
|---|---|---|
| **Contained** | Affects only this decision/system, no spillover | Slightly slower API response time |
| **Team** | Affects the immediate team's work or workload | Engineering time diverted to manage vendor integration |
| **Company** | Affects company-wide metrics, strategy, or runway | Margin compression affecting overall unit economics |
| **External** | Affects people or entities outside the company — customers, investors, partners, regulators | Compliance objections from enterprise customers; investor confidence in a fundraising narrative |

---

## The Priority Flag

**Any node tagged `irreversible` + (`company` or `external`) blast radius is a priority flag**, regardless of its depth or confidence percentage. Surface these explicitly and prominently in the final output, even if they're a low-confidence depth-4 speculative node. The logic: a 15%-likely irreversible company-wide event deserves more attention than a 90%-likely contained, easily-reversible inconvenience. Probability-weighted severity, not raw probability, is what should drive attention.

---

## Quick Tagging Reference Table

Use this shape when tagging each node in the output:

```
Node: [causal claim]
Type: [mechanical / behavioral / speculative]
Confidence: [X]% (conditional on parent)
Reversibility: [easy / costly / irreversible]
Blast radius: [contained / team / company / external]
⚠️ PRIORITY FLAG if irreversible + company/external
```

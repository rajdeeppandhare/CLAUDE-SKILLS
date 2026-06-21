# Tree Generation Playbook

Method for building branching, multi-hop consequence trees from a single decision.

---

## The Branching Rule

A decision tree's value comes from showing the **fan-out**, not from picking the single most interesting story. At every node, ask: "What are the genuinely distinct plausible effects of this, not just the most dramatic one?"

**Minimum branching per node:**
- First-hop (depth 1): 2–4 branches from the root decision
- Subsequent hops (depth 2+): 1–3 branches per parent node

If a node truly has only one plausible downstream effect, that's fine — don't force false branches. But check first: most real consequences fork because they affect multiple stakeholders, multiple timeframes, or multiple systems at once.

### How to find the forks
For any node, check whether it affects, independently:
- **Cost/resources** (money, time, attention)
- **People/relationships** (team, customers, investors, partners)
- **Capability/position** (what becomes possible or impossible afterward)
- **Perception** (how this is read internally or externally)

If a node touches 2+ of these categories, it almost always branches into at least 2 distinct downstream paths — one per category affected.

---

## Depth Guidance by Decision Horizon

| Decision horizon (from Phase 1) | Useful tree depth | Reasoning |
|---|---|---|
| Days to weeks | 2–3 hops | Short horizon, fewer compounding effects worth modeling |
| Months | 3–4 hops | Standard range for most founder/business decisions |
| Years | 4–5 hops, but flag depth 4+ as highly speculative | Long horizon decisions have more room for compounding chains, but confidence drops fast — don't pad with low-value speculation |

**Stop generating depth once confidence drops below ~10% or the chain requires stacking more than 4–5 independent assumptions.** Past that point, say explicitly: "Beyond this point, the chain is too speculative to usefully map — too many compounding assumptions."

---

## Worked Example: Branching vs. Linear

**Bad (linear, narrative, picks one path):**
```
Use OpenAI API → Higher costs → Lower margins → Harder fundraising → Company struggles
```
This is a single cherry-picked doom narrative. It hides the fact that "higher costs" doesn't only lead to lower margins, and ignores any positive branches entirely.

**Good (branching, shows the real fan-out):**
```
Use OpenAI API
├─→ Higher per-unit costs
│     ├─→ Lower margins (if pricing doesn't change)
│     │     ├─→ Harder enterprise sales conversations
│     │     └─→ Slower path to profitability
│     └─→ Price increase passed to customer (if margins protected)
│           └─→ Risk of losing price-sensitive segment
├─→ Vendor lock-in
│     └─→ Pricing power shifts to OpenAI at renewal
├─→ Faster time-to-market (no need to build/train in-house)
│     ├─→ Earlier customer feedback
│     └─→ First-mover advantage in the category
└─→ Customer data leaves internal infrastructure
      └─→ Enterprise compliance objections
            └─→ Longer sales cycles for regulated customers
```
Same starting decision, but this shows multiple independent dimensions branching simultaneously — cost, vendor relationship, speed, and compliance — instead of one linear doom (or optimism) story.

---

## Node Phrasing Standards

- Each node should be a single, concrete causal claim — not a vague category.
  - Bad: "Business impact"
  - Good: "Harder enterprise sales conversations due to thinner margins"
- Each node should name the mechanism, not just the outcome, where possible.
  - Bad: "Things get harder"
  - Good: "Fundraising narrative weakens because unit economics look worse to investors"
- Avoid stacking multiple causal claims into a single node — split them into separate nodes if more than one mechanism is doing the work.

---

## When to Stop Branching a Path

Stop extending a specific branch (even if the overall tree continues elsewhere) when any of these is true:
- Confidence has decayed below ~10%
- The node has crossed into a domain with no further visibility (e.g. "and then geopolitics shifts" — too far outside what's tractable to reason about)
- The node is now describing an effect so diffuse it's not actionable (e.g. "general uncertainty increases")

It's fine — expected, even — for different branches of the same tree to terminate at different depths. Not every path is equally rich.

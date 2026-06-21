# Feedback Loop Patterns

How to detect and classify loops in a consequence tree — where a downstream node feeds back into an earlier cause.

---

## Why Loops Matter More Than Linear Chains

A tree is a one-time cost or benefit. A loop is a *dynamic* — it compounds or self-corrects over time, which is usually the single most important structural insight in the whole map. Most people intuitively model decisions as trees (this happens, then that happens) and miss loops entirely, which is exactly why this scan exists as a dedicated phase.

---

## Loop Type 1 — Reinforcing Loop

**Definition:** A downstream effect strengthens the condition that caused it, creating a compounding spiral (positive feedback in the systems-theory sense — "positive" meaning amplifying, not meaning good).

**Pattern shape:**
```
A → B → C → (back to) A, but stronger
```

**Worked example:**
```
Higher API costs
  → Lower margins
    → Harder fundraising story
      → Less capital raised
        → Can't invest in cost-reduction engineering
          → Stays dependent on expensive API
            → (loops back to) Higher API costs, now structurally locked in
```

This is the most dangerous pattern in any consequence tree because it means the problem gets *worse* over time without new intervention, not just persists. Flag reinforcing loops as the highest-priority structural finding, above almost any single node — a loop changes the entire risk profile of the decision from "one-time cost" to "structural trap."

**What to look for:** scan every leaf and mid-tree node and ask "does this, even loosely, make the original triggering condition more likely or more severe?" If yes, trace the path back and draw the loop explicitly.

---

## Loop Type 2 — Self-Correcting Loop

**Definition:** A downstream effect weakens or counteracts the condition that caused it, creating natural damping (negative feedback in the systems-theory sense — "negative" meaning dampening, not meaning bad).

**Pattern shape:**
```
A → B → C → (back to) A, but weaker
```

**Worked example:**
```
Higher API costs
  → Price increase passed to customers
    → Some price-sensitive customers churn
      → Remaining customer base is less price-sensitive
        → Pricing power increases
          → (loops back to) Higher costs become easier to absorb
```

Self-correcting loops are good news structurally — they mean the system has some natural resistance to the problem compounding. Still worth flagging, but with a different tone: "this effect is likely to stabilize on its own, here's the mechanism," rather than "this requires urgent intervention."

---

## How to Scan for Loops

After the full tree (both the real decision and counterfactual branches) is generated:

1. List every node in the tree.
2. For each node beyond depth 1, ask: "Could this plausibly affect any ancestor node in its own path, or any node in a sibling branch?"
3. If yes, trace the connection and classify as reinforcing or self-correcting using the definitions above.
4. Note the **loop closure depth** — how many hops it takes for the loop to complete. Shorter loops (2–3 hops) compound faster and are usually more urgent than long loops (5+ hops).

**Don't force loops that aren't there.** Many decision trees genuinely have no meaningful loop — the effects are real but don't feed back into the original cause. In that case, state explicitly: "No significant feedback loops detected in this tree — the consequences are real but don't appear to compound back into the original decision."

---

## Cross-Branch Loops

Loops aren't only within a single branch — a node in one branch can feed back into a different branch entirely. These are easy to miss because they require holding the whole tree in mind at once, not just following one path top to bottom. Specifically check:

- Does a node in the "cost" branch affect a node in the "speed" or "compliance" branch?
- Does a node in the counterfactual (inaction) tree actually depend on something that happens in the real-decision tree, or vice versa? (This is rarer but worth a quick check — usually the two trees are independent, but not always.)

---

## Output Format for Loops

```
🔄 LOOP DETECTED: [Reinforcing / Self-Correcting]
Path: [Node A] → [Node B] → [Node C] → back to [Node A]
Closure depth: [N] hops
Why it matters: [one sentence — compounding risk, or natural stabilization]
```

If this is a reinforcing loop touching a priority-flagged node (irreversible + company/external blast radius from the tagging rubric), say so explicitly — that combination is the single most urgent finding the skill can produce.

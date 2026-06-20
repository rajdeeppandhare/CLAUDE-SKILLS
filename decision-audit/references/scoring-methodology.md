# Scoring Methodology

The mechanics for separating decision quality from outcome quality, and for classifying the outcome driver honestly.

---

## The Core 2×2

Every closed-out decision lands in exactly one of four quadrants:

```
                    GOOD OUTCOME           BAD OUTCOME
                  ┌─────────────────────┬─────────────────────┐
GOOD DECISION     │   ✅ DESERVED WIN    │    🍀 BAD LUCK       │
(sound reasoning) │   Reasoning was      │   Reasoning was      │
                  │   sound; outcome     │   sound; outcome     │
                  │   confirms it        │   was unlucky        │
                  ├─────────────────────┼─────────────────────┤
BAD DECISION      │   🎲 LUCKY BREAK     │   ❌ DESERVED LOSS    │
(flawed reasoning)│   Reasoning was      │   Reasoning was      │
                  │   flawed; got        │   flawed; outcome    │
                  │   away with it       │   confirms it        │
                  └─────────────────────┴─────────────────────┘
```

**The two quadrants that matter most for learning are the diagonals you'd normally ignore:**
- **Bad Luck** (good decision, bad outcome) → Do NOT change behavior here. The instinct is to abandon a sound process because it failed once. Resist it.
- **Lucky Break** (bad decision, good outcome) → Do NOT reinforce this. The instinct is to repeat what worked. That's exactly how systematic errors get baked in.

If pattern analysis later shows a cluster of Lucky Breaks, that's the highest-priority finding — it means the person's current success is hiding a reasoning flaw that will eventually catch up with them.

---

## Scoring Decision Quality (Independent of Outcome)

Score BEFORE looking at the outcome. Use these four checks:

### Check 1 — Were genuine alternatives considered?
- **Pass**: At least one real alternative was weighed, with actual reasons it was rejected
- **Fail**: The "decision" was actually a default action, or alternatives were strawmen never seriously considered

### Check 2 — Was confidence calibrated to actual uncertainty?
- **Pass**: Confidence level reflects the real information available — not artificially high because the person wanted it to be true, not artificially low from anxiety
- **Fail**: Confidence doesn't match the information state (e.g. "9/10 confident" on something with no validating data)

### Check 3 — Was the key assumption reasonable given available information?
- **Pass**: The assumption was the best bet given what could have been known at the time, even if it turned out wrong
- **Fail**: The assumption ignored available, accessible information that would have changed the call

### Check 4 — Was deliberation time proportional to reversibility?
- **Pass**: Irreversible/high-stakes decisions got proportionally more deliberation; reversible/low-stakes ones weren't over-deliberated
- **Fail**: A one-way-door decision was rushed, or a two-way-door decision was stalled on for an unreasonable amount of time

**Verdict rule:** A decision is scored **"good"** if it passes 3 or 4 of these checks. It's scored **"bad"** if it passes 2 or fewer. Note which specific checks failed — that detail feeds the pattern analysis later, especially the assumption-failure clustering.

---

## Classifying the Outcome Driver

This is the field that prevents the whole system from collapsing into "good things happened" vs. "bad things happened." Use this decision tree:

```
Did the outcome happen primarily because of something the person
controlled or could have reasonably foreseen?
│
├── YES → Was their reasoning/assumption about it correct?
│         ├── YES → outcome_driver = reasoning_correct
│         └── NO  → outcome_driver = reasoning_incorrect
│
└── NO  → Was it primarily an external factor outside their control?
          (market shift, third party's unrelated action, unforeseeable event)
          ├── YES → outcome_driver = external_factor
          └── Both factors contributed meaningfully → outcome_driver = mixed
```

**Common misclassification to watch for:** the person blaming external factors for outcomes that were actually within their control (self-serving bias), or crediting their own genius for outcomes that were actually lucky external breaks (also self-serving bias, inverted). Push back gently if the outcome-driver classification looks like it's protecting the ego rather than describing reality. Ask: "If a stranger looked at this situation with no emotional stake, would they call this external or within your control?"

---

## Combining Into the Final Quadrant

```
decision_quality_verdict = "good"  AND outcome was positive  → deserved_win
decision_quality_verdict = "good"  AND outcome was negative  → bad_luck
decision_quality_verdict = "bad"   AND outcome was positive  → lucky_break
decision_quality_verdict = "bad"   AND outcome was negative  → deserved_loss
```

"Outcome was positive/negative" is a separate judgment from the quality verdict — ask the user directly whether the outcome was net positive, net negative, or mixed/unclear (in which case note it as inconclusive rather than forcing a quadrant).

---

## The "Would Decide Same Again" Cross-Check

After scoring the quadrant, ask the would-decide-same-again question independently. It should usually agree with the quadrant, but disagreement is itself informative:

| Quadrant | Expected answer | If it disagrees... |
|---|---|---|
| Deserved win | Yes | Rare — worth asking why they'd change it despite the win |
| Bad luck | Yes | If they say "no," they may be conflating outcome with quality — redirect to Check 1–4 |
| Lucky break | No | If they say "yes," they're rationalizing a flawed process because it happened to work — flag this explicitly, it's a high-value finding |
| Deserved loss | No | Expected agreement |

A "Lucky Break" decision where the person says they'd do it again is the single highest-priority flag in this entire system — it means a known-flawed process is about to get reinforced and repeated.

---

## Minimum Evidence Standard

Don't force a quadrant if the data doesn't support it:
- If `outcome_driver` is missing or the user can't articulate it confidently, don't compute `decision_quality_verdict` — ask for the missing context first.
- If the outcome is genuinely still unclear/mixed, log it as `outcome_quadrant: inconclusive` rather than guessing. It's fine to leave a decision in limbo for another review cycle.

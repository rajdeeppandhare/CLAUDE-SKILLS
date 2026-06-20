# Pattern Analysis Playbook

Five scans to run across a closed-decision log (5+ entries minimum). Each scan answers a question no single decision can answer alone.

---

## Scan 1 — Calibration

**Question answered:** When the person says they're X% confident, are they actually right X% of the time?

### Method
1. Bucket all closed decisions by stated `confidence` (1–10). Group into ranges if the sample is small: Low (1–4), Medium (5–7), High (8–10).
2. For each bucket, compute the **actual hit rate** — the percentage where `decision_quality_verdict` was "good" AND outcome was positive (i.e., deserved wins as a share of that bucket).
3. Compare stated confidence to actual hit rate.

### Interpretation
| Pattern | What it means |
|---|---|
| High confidence (8–10), hit rate ~80-100% | Well calibrated at the high end |
| High confidence (8–10), hit rate <60% | **Systematic overconfidence** — the person's gut sense of certainty is not trustworthy and should be discounted, especially for high-stakes irreversible calls |
| Medium confidence (5–7), hit rate >85% | **Systematic underconfidence** — the person hedges more than the data justifies; may be leaving good decisions un-pursued out of unwarranted caution |
| Flat hit rate across all confidence buckets (~50% everywhere) | Confidence isn't tracking anything real — it's noise, not signal. This is a more serious finding than over/underconfidence because it means the stated number isn't doing any work |

**Minimum sample per bucket:** Don't report a calibration finding for a bucket with fewer than 3 entries. Say "insufficient data in the 8-10 confidence range" rather than computing a misleading percentage from 1-2 data points.

---

## Scan 2 — Domain Skew

**Question answered:** Is the person reliably good at some categories of decision and reliably bad at others?

### Method
1. Group closed decisions by `domain` tag.
2. For each domain with 3+ entries, compute the "good decision" rate (`decision_quality_verdict = good` as a share of that domain's entries).
3. Rank domains from highest to lowest good-decision rate.

### Interpretation
- A domain sitting meaningfully below the person's overall average (e.g. 20+ percentage points lower) is a **delegation signal**, not a "try harder" signal. The recommendation should be to get a second opinion, bring in someone else's judgment, or build in a forced pause/checklist for that domain — not just "be more careful."
- A domain sitting meaningfully above average is worth naming explicitly — it tells the person where to trust their own snap judgment.
- If domains have too few entries to compare (under 3 each), say so and suggest tagging more consistently going forward rather than forcing a conclusion.

---

## Scan 3 — Speed Bias

**Question answered:** Do fast decisions outperform slow ones, or the reverse?

### Method
1. Split closed decisions into `rushed` vs `deliberate` (from `time_pressure`).
2. Compute good-decision rate for each group.
3. Compare.

### Interpretation
| Pattern | What it means |
|---|---|
| Deliberate rate notably higher than rushed | Time pressure is hurting decision quality — build in more buffer before high-stakes calls when possible |
| Rushed and deliberate rates are similar | Extra deliberation time isn't buying better decisions — possible overthinking; the person may be able to decide faster without a quality cost, especially on reversible decisions |
| Rushed rate notably higher than deliberate | Counterintuitive but real for some people — may indicate analysis paralysis or second-guessing during long deliberation that erodes a good initial read. Worth flagging explicitly since it cuts against conventional advice |

---

## Scan 4 — Assumption Failure Clustering

**Question answered:** Is there a specific, recurring type of wrong assumption — not just "bad at deciding" in general?

### Method
1. Pull the `key_assumption` field from every decision classified as `lucky_break` or `deserved_loss` (the two quadrants where the decision quality itself was bad).
2. Read across them for a repeated *type* of assumption failure. Don't just look for literal repeated words — look for the underlying pattern. Common categories:
   - Assumed demand/interest without validating it directly
   - Assumed a third party would follow through without confirming
   - Assumed a timeline would hold despite known risk factors
   - Assumed information was complete when it wasn't checked
   - Assumed past success would generalize to a new context
3. Name the cluster specifically, with at least 2 supporting examples from the log.

### Interpretation
A single bad assumption is a one-off. **Two or more decisions sharing the same assumption-failure type is a blind spot** — and it's the single most actionable output of this whole skill, because it's specific enough to build a checklist item around (e.g., "before committing to a launch date, get one external confirmation that the timeline is realistic").

**Do not force a cluster from a single example.** If only one decision shows a given assumption type, mention it as a one-off worth watching, not a confirmed pattern.

---

## Scan 5 — Reversibility Mismatch

**Question answered:** Is deliberation effort actually allocated according to how much each decision matters?

### Method
1. Cross-tabulate `decision_type` (reversible/irreversible) against `time_pressure` (rushed/deliberate).
2. Look for mismatches:
   - Irreversible + rushed → high-risk combination
   - Reversible + deliberate (especially repeatedly) → possible stalling pattern

### Interpretation
- A cluster of **irreversible decisions made under time pressure** is the highest-risk finding in this scan — irreversible mistakes can't be corrected, so rushing them is the costliest version of any bias in this whole system. Flag prominently even from a small number of examples (this scan's threshold is lower than the others — 2+ instances is worth naming).
- A cluster of **reversible decisions getting heavy deliberation** suggests the person may be treating low-stakes, correctable decisions with the same weight as high-stakes ones — a time/energy misallocation rather than a quality problem per se.

---

## Synthesis Rule

After running all five scans, identify the **single headline finding** — the one pattern that is both most strongly evidenced (largest sample, clearest signal) and most actionable (translates into a specific behavior change). Lead the report with this, not with a wall of every scan's output. The other scans support the report; only one or two should be the take-home message.

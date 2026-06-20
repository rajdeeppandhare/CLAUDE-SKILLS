# Decision Log Schema

The full field set for logging a decision. Captured in two passes: at decision time (Phase 1) and at review time (Phase 3). Never backfill decision-time fields after the outcome is known — that defeats the purpose.

---

## Pass 1 — Captured At Decision Time

| Field | Type | Description | Why it matters |
|---|---|---|---|
| `id` | string | Short unique identifier (e.g. `D-001`) | Lets you reference decisions across sessions |
| `date_logged` | date | When the decision was made | Anchors the log chronologically |
| `decision` | string (1 sentence) | What was chosen | The atomic unit of the log |
| `alternatives_considered` | list | Genuine alternatives that were on the table | Without this you can't tell a real decision from a default action |
| `confidence` | integer 1–10 | How confident at the time the decision would work out | The core calibration input — this is graded against actual outcomes later |
| `key_assumption` | string | The single belief this decision is betting on | The thing that gets graded later; also the unit for blind-spot clustering |
| `decision_type` | enum: `reversible` / `irreversible` | Bezos one-way-door vs two-way-door framing | Deliberation effort should scale with this; mismatches are a pattern signal |
| `time_pressure` | enum: `rushed` / `deliberate` | Was there real time to think, or was this forced | Used in the speed-bias scan |
| `domain` | string (free tag) | e.g. `technical`, `product`, `hiring`, `financial`, `strategic`, `personal` | Used in the domain-skew scan |
| `expected_review_date` | date | When the outcome will likely be knowable | Drives the "pending review" nudge list |
| `stakes` | enum: `low` / `medium` / `high` | Rough magnitude of consequence | Optional but useful for weighting pattern analysis |

---

## Pass 2 — Captured At Review Time (Phase 3)

Only filled in once the outcome is actually knowable. Never estimated in advance.

| Field | Type | Description | Why it matters |
|---|---|---|---|
| `outcome` | string | What actually happened, in concrete terms | The raw outcome data |
| `outcome_driver` | enum: `reasoning_correct` / `reasoning_incorrect` / `external_factor` / `mixed` | Was the outcome caused by the reasoning being right/wrong, or by something outside the person's control | The single most important field in the whole schema — see scoring-methodology.md for the decision tree |
| `would_decide_same_again` | enum: `yes` / `no` / `partially` | Knowing ONLY what was knowable at decision time, would they choose the same way | Isolates decision quality from outcome luck |
| `decision_quality_verdict` | enum: `good` / `bad` | Scored using the 2×2 from scoring-methodology.md, BEFORE looking at outcome valence | The graded output |
| `outcome_quadrant` | enum: `deserved_win` / `bad_luck` / `lucky_break` / `deserved_loss` | The 2×2 cell this decision lands in | The combined verdict |
| `review_date_actual` | date | When the review actually happened | For tracking how long outcomes took to materialize vs. expected |
| `notes` | string (optional) | Anything else worth capturing | Free text for context that doesn't fit elsewhere |

---

## Example Entry (Pass 1, at decision time)

```
id: D-014
date_logged: 2026-03-02
decision: Launch MoldMind's mobile app before the web dashboard is feature-complete
alternatives_considered:
  - Delay mobile launch until web parity reached
  - Launch mobile as a stripped-down companion app only
confidence: 7
key_assumption: Early users will tolerate a mobile-first experience even if some
  dashboard features are missing, because mobile is their primary use case
decision_type: reversible
time_pressure: deliberate
domain: product
expected_review_date: 2026-05-01
stakes: medium
```

## Example Entry (Pass 2, after review)

```
outcome: Mobile launch shipped on time; 40% of early users churned within 2 weeks
  citing missing dashboard features they expected to also use on mobile
outcome_driver: reasoning_incorrect
would_decide_same_again: no
decision_quality_verdict: bad
outcome_quadrant: deserved_loss
review_date_actual: 2026-05-03
notes: The key assumption was wrong — users wanted parity, not a mobile-first
  experience. Worth checking if this same assumption shows up in other product
  decisions.
```

---

## Storage Format Notes

The user may want this logged as:
- A markdown table appended to a running document
- A JSON array (better for programmatic pattern analysis later)
- Entries pasted directly into chat for one-off review

Ask which format they're using if it's not clear from context. If they're starting fresh, default to a markdown table for readability, and offer JSON if they mention wanting to analyze the log programmatically (e.g. in a spreadsheet or script) later.

**Always preserve every field**, even if a value is "none" or "n/a" — missing fields silently break the pattern-analysis scans in Phase 5.

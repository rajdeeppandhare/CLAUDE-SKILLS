# Report Templates

Output formats for each phase. Fill every field — no placeholders left in final output.

---

## Template 1 — Single Decision Logged (Phase 1 output)

```
📝 LOGGED: [decision in one line]

ID: [D-0XX]
Domain: [tag]
Type: [reversible / irreversible]
Confidence: [X]/10
Time pressure: [rushed / deliberate]

Key assumption: [the bet this decision is making]

Alternatives considered:
• [alt 1]
• [alt 2]

Review by: [expected_review_date]

──────────────────────────────────────
Saved to your log. I'll flag this for review around [date], or sooner if you
bring it back up.
```

---

## Template 2 — Single Decision Reviewed (Phase 3–4 output)

```
🔍 REVIEWING: [decision in one line]   (logged [date_logged])

WHAT HAPPENED
[outcome, stated plainly]

DECISION QUALITY CHECK (scored before looking at outcome)
✓/✗ Genuine alternatives considered
✓/✗ Confidence calibrated to actual uncertainty
✓/✗ Key assumption reasonable given available info
✓/✗ Deliberation proportional to reversibility

Verdict: [GOOD / BAD] decision (passed [N]/4 checks)

OUTCOME DRIVER
[reasoning_correct / reasoning_incorrect / external_factor / mixed]
[One sentence explaining the classification]

QUADRANT: [✅ Deserved Win / 🍀 Bad Luck / 🎲 Lucky Break / ❌ Deserved Loss]

Would decide the same way again (knowing only what you knew then)? [Yes/No/Partially]

──────────────────────────────────────
[One sentence of interpretation — what this single decision does or doesn't
tell you. If it's a Lucky Break with "would decide again = yes," flag this
explicitly as high-priority — see scoring-methodology.md.]
```

---

## Template 3 — Pending Review Nudge (Phase 2 output)

```
📋 DECISIONS AWAITING REVIEW

[N] decisions have passed their expected review date:

• [D-0XX] — [decision, one line] — expected [date], now [X days] overdue
• [D-0XX] — [decision, one line] — expected [date], now [X days] overdue

Want to close any of these out now?
```

---

## Template 4 — Full Pattern Report (Phase 5–6 output)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DECISION QUALITY REPORT
[date range] · [N] decisions reviewed
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

HEADLINE FINDING
[The single most actionable pattern, one sentence, stated plainly —
not hedged, not generic]

──────────────────────────────────────
CALIBRATION

Stated confidence vs. actual hit rate:

Confidence band    Hit rate    Sample size
8–10 (high)         [X]%        [n]
5–7 (medium)        [X]%        [n]
1–4 (low)           [X]%        [n]

[One sentence naming the gap, if any — e.g. "You call yourself 8/10 confident
in situations that pan out about 55% of the time — your gut is running about
two points hot."]

──────────────────────────────────────
DOMAIN BREAKDOWN

Domain          Good-decision rate    Sample size
[domain 1]       [X]%                  [n]
[domain 2]       [X]%                  [n]
[domain 3]       [X]%                  [n]

Trust your own call on: [domain(s) with highest rate]
Get a second opinion on: [domain(s) with lowest rate]

──────────────────────────────────────
NAMED BLIND SPOT(S)

[Blind spot 1, stated specifically]
Evidence: [D-0XX], [D-0XX][, D-0XX]
Suggested checkpoint: [one concrete, specific intervention — e.g. "before
committing to a launch date, get one external confirmation the timeline holds"]

[Blind spot 2, if found — same structure]

──────────────────────────────────────
SPEED & REVERSIBILITY

[One to two sentences combining the speed-bias and reversibility-mismatch
scans — only include findings that cleared the evidence threshold]

──────────────────────────────────────
LUCKY BREAKS TO WATCH

[If any decisions landed in the Lucky Break quadrant with "would decide
again = yes," list them here explicitly — this is the highest-priority
risk in the whole report]

• [D-0XX] — [one line] — flawed reasoning that happened to work; if repeated,
  expect it to eventually fail

──────────────────────────────────────
STILL PENDING REVIEW

• [D-0XX] — [decision] — expected [date]
• [D-0XX] — [decision] — expected [date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Formatting Rules

- **Every percentage needs a sample size next to it.** A "73% good-decision rate" from 3 decisions is not the same claim as 73% from 30 — always show `(n=X)`.
- **Never state a pattern-analysis finding (Scans 1–5) without naming which decision IDs support it.** This keeps every claim traceable back to the actual log, not a vibe.
- **The headline finding must be a single sentence with a clear behavior implication** — not "you have mixed results across domains," but "your technical calls are reliable (78% good) while your hiring calls are not (35% good) — bring in a second reviewer for hiring decisions specifically."
- **Don't use the word "always" or "never"** unless the sample size genuinely supports it (5+ instances with no exceptions). Otherwise use "tends to" or state the actual rate.

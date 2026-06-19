# Output Template

Use this template for every gap that scores ≥5.0. Fill every field — blank fields are not allowed.

---

## Individual Gap Brief

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GAP #[N]: [One-line name — who is underserved and how]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Score: [X.X]/10
Layer: [Surface / Segment / Negative-Space]
Segment: [The buyer persona or market segment this gap targets]

EVIDENCE
────────
• [Data point 1 — specific, with source: "14 reviews on G2 mention X"]
• [Data point 2 — specific, with source: "None of A/B/C job postings mention Y"]
• [Data point 3 — specific, with source: "Pricing floor at $X/mo excludes Z segment"]
• [Data point 4 — optional; add if available]

WHY IT'S OPEN
─────────────
[One paragraph explaining the structural reason incumbents haven't addressed this.
Not "they missed it" — explain the pricing lock-in, architectural constraint,
sales-motion mismatch, or GTM assumption that keeps them away. This is the moat
analysis for a new entrant.]

WEDGE THESIS
────────────
[One to three sentences: the specific positioning move a new entrant makes.
Be concrete — name the price point, persona, or capability that creates the wedge.
Not "build a simpler product" — "a self-serve, $49/mo, SMB-first version targeting
exactly the complexity complaint in G2 reviews, with a 14-day trial that requires
no credit card."]

COUNTER-RISK
────────────
[The main reason this could be a graveyard — and how to verify before acting.
Format: "[Risk] — verify by [specific research step]."
Example: "SMB usage-based pricing has a known CAC>LTV failure mode — verify by
checking whether any SMB-focused competitors in adjacent categories show sustainable
unit economics, or by running a low-budget paid acquisition test before committing."]

VALIDATION STEPS
────────────────
1. [First thing to verify before committing — most important]
2. [Second validation step]
3. [Optional third step if high-risk gap]
```

---

## Summary Matrix (End of Document)

After all individual briefs, output this summary matrix:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OPPORTUNITY SUMMARY MATRIX
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Rank  Gap Name                          Score   Layer           Recommendation
────  ────────────────────────────────  ──────  ──────────────  ──────────────
**1   [Gap name]                        X.X     Negative-Space  ACT**
**2   [Gap name]                        X.X     Segment         ACT**
**3   [Gap name]                        X.X     Segment         VALIDATE**
  4   [Gap name]                        X.X     Surface         MONITOR
  5   [Gap name]                        X.X     Surface         MONITOR

Recommendation key:
  ACT       → Score ≥7.0; sufficient evidence to invest in exploration
  VALIDATE  → Score 5.0–6.9; worth a 2-4 week validation sprint before committing
  MONITOR   → Score 4.0–4.9; real signal but insufficient evidence or elevated risk
```

---

## Strategic Synthesis (Final Section)

After the matrix, add a three-part synthesis:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STRATEGIC SYNTHESIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

FASTEST WEDGE: [Gap #N]
Why: [One sentence — existing demand, low switching cost, or distribution clarity]
First move: [Specific action — "run a $500 paid test targeting [keyword/persona]"]

MOST DEFENSIBLE LONG-TERM: [Gap #N]
Why: [One sentence — structural, architectural, or regulatory moat]
Risk: [The one thing that could close this window]

HIGHEST RISK / WATCH: [Gap #N]
Why: [One sentence — real signal but elevated graveyard or validation risk]
Trigger: [What evidence would make this an "ACT" recommendation]

───────────────────────────────────────────────────
BOTTOM LINE

Your top bet based on this analysis is Gap #[N] because [reason].
Before acting, verify [counter-risk] by [specific research step].

Confidence: [Low / Medium / High] based on [evidence quality note].
```

---

## Formatting Notes

- **Never use vague language in evidence bullets.** "Reviews mention complexity" → WRONG. "11 reviews on G2 between Jan–Oct 2024 use the word 'complex' or 'overwhelming' in the context of onboarding" → RIGHT.
- **Wedge thesis must name a price, persona, or capability.** Vague positioning is not a thesis.
- **Counter-risk must include a verification method.** "This might not work" is not a counter-risk.
- **Maximum 5 primary briefs.** If you found more gaps, score them and cut. The matrix shows all scored gaps; the briefs only cover the top 5.
- **Top 3 in summary matrix are always bolded.** These are the decision-ready recommendations.

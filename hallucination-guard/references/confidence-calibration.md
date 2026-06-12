# Confidence Calibration — Reference

Used in Stage 7 of the Hallucination Guard pipeline.

Good confidence scoring means being neither overconfident nor underconfident.
This file provides calibration guidance so scores are consistent and meaningful.

---

## The Tiers

| Tier | Score Range | Label | Visual |
|------|------------|-------|--------|
| 1 | 95–100% | Verified | ✅ |
| 2 | 80–94% | Strongly Supported | 🟢 |
| 3 | 50–79% | Plausible | 🟡 |
| 4 | 0–49% | Speculative | 🔴 |

---

## Tier 1 — Verified (95–100%)

**Criteria:**
- Directly stated by the user in the current prompt
- Mathematical or logical fact with a proof path
- Definitional / taxonomic claim (e.g. "TypeScript is a superset of JavaScript")
- Well-established scientific consensus

**Examples:**
- "Python uses indentation for block scoping" → 99%
- "The user said their database is Supabase" (user said it) → 98%
- "2 + 2 = 4" → 100%

**Presentation:** Usually no need to show the score unless asked. State the claim directly.

---

## Tier 2 — Strongly Supported (80–94%)

**Criteria:**
- Reasonable inference from provided context
- General knowledge that is stable but not definitional
- Industry-standard practice or widely documented pattern

**Examples:**
- "Next.js apps typically use API routes for backend logic" → 88%
- "A 500 error usually indicates a server-side failure" → 90%
- "React hooks were introduced in React 16.8" → 93%

**Presentation:** State with mild hedging ("typically", "generally"). Show score if
the user is making a significant decision based on it.

---

## Tier 3 — Plausible (50–79%)

**Criteria:**
- Educated inference with meaningful uncertainty
- Depends on unstated variables
- Based on patterns that admit many exceptions
- General knowledge about a fast-changing domain

**Examples:**
- "The startup likely has fewer than 10 engineers" (inferred from context) → 60%
- "The performance issue could be N+1 queries" (one of several possibilities) → 65%
- "GPT-5 is probably already being trained" → 70%

**Presentation:** Always hedge explicitly. Show confidence tier. Consider listing
alternatives (Stage 6).

---

## Tier 4 — Speculative (0–49%)

**Criteria:**
- Minimal evidence
- Future event prediction
- Unique-to-user situation with no context provided
- Rapidly changing domain where training data may be stale

**Examples:**
- "GPT-6 will release in 2025" → 20%
- "Anthropic's revenue is approximately $X" (no source) → 15%
- "Your bug is caused by a race condition" (no logs/code) → 30%

**Presentation:** Always label 🔴 Speculative. Explain why uncertainty is high.
Encourage the user to verify independently.

---

## Common Calibration Mistakes

### Overconfidence

❌ "The issue is a memory leak." (stated as fact, no code seen)
✅ "🟡 Plausible (60%) — A memory leak is one possible cause. Without heap profiling
   data I can't confirm it."

### Underconfidence (also a problem)

❌ "Python *might* be a programming language, but I could be wrong."
✅ "Python is a programming language." (✅ Verified — no caveat needed)

### False Precision

❌ Listing a score of "73%" implies measurement accuracy that doesn't exist.
✅ Use the four tiers. They communicate meaningful uncertainty without fake precision.

---

## When to Show Scores

| Situation | Show scores? |
|-----------|-------------|
| All claims are Tier 1 (Verified) | No — clean answer is better |
| User explicitly asked for confidence | Yes — all claims |
| Mixed tiers in one answer | Yes — on Tier 3/4 claims |
| High-Risk domain (legal, medical, etc.) | Yes — always |
| Debugging / architecture recommendation | Yes — on key claims |
| Simple factual question, one verified fact | No |

---

## Inline Score Format

When showing scores, keep them compact and inline:

```
The bottleneck is likely in your database query layer. 🟡 Plausible (65%) — based
on the symptoms you described; profiling would confirm this.
```

Not a separate block unless the user asked for a full confidence breakdown.

# Evidence Audit — Reference

Used in Stage 2 of the Hallucination Guard pipeline.

---

## The Audit Format

For each significant claim in a draft answer, run:

```
Claim:   <exact claim being made>
Source:  [User-provided | Conversation memory | General knowledge | Assumption]
Verdict: ✅ Keep | ❌ Remove | ⚠️ Flag
```

---

## Source Hierarchy

| Source | Trust Level | Action |
|--------|-------------|--------|
| User-provided in this prompt | Highest | Accept, but verify logic |
| Prior message in conversation | High | Accept, check for updates |
| Claude general knowledge (pre-cutoff) | Medium | State uncertainty if recent |
| Inference from context | Low | Flag as assumption |
| Claude-invented detail | None | Remove immediately |

---

## Worked Examples

### Example 1 — Startup Tech Stack

**User prompt:** "How should I scale my startup?"

**Draft claim (before audit):** "Since you're using Stripe for payments, you'll want to…"

**Audit:**
```
Claim:   User is using Stripe
Source:  Assumption — user never mentioned it
Verdict: ❌ Remove
```

**Fixed:** "Depending on your payment provider, you'll want to…"

---

### Example 2 — Company Information

**User prompt:** "Tell me about Anthropic's revenue."

**Draft claim (before audit):** "Anthropic reported $200M ARR in 2024."

**Audit:**
```
Claim:   Anthropic reported $200M ARR in 2024
Source:  General knowledge — unverifiable exact figure, training data cutoff
Verdict: ⚠️ Flag as uncertain
```

**Fixed:** "Anthropic's exact revenue figures are not publicly confirmed. Reports suggest
significant ARR growth but specific numbers vary by source. Confidence: 🔴 Speculative."

---

### Example 3 — Code Debugging

**User prompt:** "My app crashes on startup."

**Draft claim (before audit):** "The issue is likely a null pointer in your database connection."

**Audit:**
```
Claim:   Null pointer in database connection
Source:  Assumption — no stack trace, no logs, no platform info provided
Verdict: ❌ Too specific to state as likely
```

**Fixed:** "Without logs or a stack trace I can't pinpoint the cause. Common startup
crash causes include: config loading errors, missing env vars, DB connection failures,
dependency version mismatches."

---

### Example 4 — Well-Evidenced Claim (Keep)

**User prompt:** "Should I use TypeScript?"

**Draft claim:** "TypeScript is a superset of JavaScript that adds static typing."

**Audit:**
```
Claim:   TypeScript is a JS superset with static typing
Source:  General knowledge — definitional, stable, well-established
Verdict: ✅ Keep — Confidence: 99%
```

---

## Red Flags — Claims That Almost Always Need Flagging

- Specific version numbers not mentioned by the user
- Pricing, revenue, user counts for any company
- Release dates for unreleased products
- Legal interpretations ("this is legal in X country")
- Medical dosages or treatment recommendations
- Claims about what a specific person said or did
- Any statistic without a named source
- Future predictions stated as near-certainties

---

## The Invention Test

Before finalising any claim, ask:

> "Did the user, prior conversation, or a source I can name provide this information —
> or did I generate it from pattern-matching?"

If the answer is "I generated it" → flag or remove.

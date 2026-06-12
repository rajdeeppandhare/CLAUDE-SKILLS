---
name: hallucination-guard
description: >
  Reasoning Integrity & Evidence Verification skill. Use this whenever the user
  asks Claude to analyse, research, investigate, or answer questions where claim
  accuracy matters — technical architecture reviews, product comparisons, medical
  or legal topics, news, company information, future predictions, debugging
  sessions, or any domain where invented facts are dangerous. Trigger on phrases
  like "verify this", "fact-check", "is this accurate", "review this answer",
  "don't hallucinate", "check your sources", "how confident are you", "analyse
  this carefully", or when the user provides context and wants a trustworthy
  synthesis. Also trigger proactively when the query touches High-Risk domains
  (legal, medical, financial, news, company-specific data, future events) even
  if the user doesn't explicitly ask for verification. Always use this skill
  before finalising answers that make specific factual claims.
---

# Hallucination Guard

A multi-layer reasoning integrity pipeline. Instead of just trying not to hallucinate,
this skill actively hunts for unsupported claims, hidden assumptions, missing evidence,
and contradictions **before** the final response is shown to the user.

---

## Pipeline Overview

Run every stage in order. Suppress internal reasoning from the final output — show
only the polished, annotated response.

```
User Query
    ↓
1. Fabrication Risk Assessment
    ↓
2. Evidence Audit
    ↓
3. Assumption Scanner
    ↓
4. Missing Information Check
    ↓
5. Contradiction Detector
    ↓
6. Alternative Explanation Generator  (skip for simple factual queries)
    ↓
7. Confidence Scoring
    ↓
8. Final Response (annotated)
```

---

## Stage 1 — Fabrication Risk Assessment

Assign a risk tier to the query before doing anything else.

| Tier | Domains | Action |
|------|---------|--------|
| **Low** | Math, programming syntax, well-established science | Normal pipeline |
| **Medium** | Business advice, product comparisons, general tech | Standard pipeline + explicit assumptions |
| **High** | Legal, medical, financial, news, company data, future events, statistics | Full pipeline + mandatory uncertainty disclosure |

> If tier = High, prepend the final response with a ⚠️ **High-Risk Domain** notice.

---

## Stage 2 — Evidence Audit

For every significant claim, trace it to a source:

```
Claim: <state the claim>
Source: [User-provided | Conversation memory | General knowledge | Assumption]
Verdict: ✅ Keep | ❌ Remove | ⚠️ Flag as uncertain
```

**Rule**: If source = Assumption and the claim is stated as fact → **remove or flag it**.

See `references/evidence-audit.md` for worked examples.

---

## Stage 3 — Assumption Scanner

List every hidden assumption before finalising.

```
Assumptions detected:
- [ ] User is using X framework
- [ ] User has Y infrastructure
- [ ] The constraint Z applies
```

If assumption count > 0:
- State them explicitly at the top of the response
- Phrase answers conditionally: *"Assuming you're using AWS…"* not *"Since you're using AWS…"*

---

## Stage 4 — Missing Information Check

Ask internally: *"What would I need to answer this with certainty?"*

If critical info is missing, **do not invent it**. Instead surface the gap:

```
⚠️ Insufficient evidence to answer fully.

To give a reliable answer I need:
1. <missing piece>
2. <missing piece>
```

Common missing info patterns are documented in `references/missing-info-patterns.md`.

---

## Stage 5 — Contradiction Detector

Compare the draft answer against:
- Current prompt
- Earlier messages in the conversation
- Known constraints and prior decisions

If a contradiction is found:

```
⚠️ Contradiction detected
Current answer conflicts with: <earlier statement>
Resolving by: <explain resolution>
```

---

## Stage 6 — Alternative Explanation Generator

*(Skip for simple factual lookups — use for debugging, analysis, recommendations)*

Generate ≥ 3 alternative explanations before committing to one. Prevents tunnel vision.

```
Possible explanations:
1. <most likely>
2. <alternative>
3. <edge case>
```

Choose the most supported one and state why.

---

## Stage 7 — Confidence Scoring

Assign a confidence tier to every major claim or section:

| Score | Label | When to use |
|-------|-------|-------------|
| 95–100% | ✅ Verified | Directly evidenced |
| 80–94% | 🟢 Strongly Supported | Well-established inference |
| 50–79% | 🟡 Plausible | Reasonable but uncertain |
| 0–49% | 🔴 Speculative | Educated guess only |

Show scores inline when they affect the user's decision. Omit for 95%+ verified claims
unless the user asked for explicit confidence markers.

---

## Stage 8 — Final Response

Structure the response as:

1. **Assumptions** (if any from Stage 3)
2. **Answer** (with confidence markers where relevant)
3. **Evidence Gaps** (if any from Stage 4)
4. **Contradictions resolved** (if any from Stage 5)
5. **Alternatives considered** (brief, if Stage 6 ran)

Use the claim classification system when helpful:

```
VERIFIED        — directly evidenced
SUPPORTED INFERENCE — reasonable deduction
SPECULATION     — possible but unconfirmed
UNKNOWN         — no evidence available
```

---

## Rewarding "I Don't Know"

The Unknown Recognition Engine is the most important part of this skill.

> Saying *"I don't know"*, *"I lack evidence"*, or *"the information provided is
> insufficient"* is **always preferred** over guessing.

Most hallucinations come from trying to be helpful when evidence is missing.
Silence the instinct to fill gaps. Surface them instead.

---

## Reference Files

| File | When to read |
|------|-------------|
| `references/evidence-audit.md` | Stage 2 worked examples and edge cases |
| `references/missing-info-patterns.md` | Common missing info by domain |
| `references/confidence-calibration.md` | Detailed confidence scoring guidance |
| `references/hallucination-taxonomy.md` | Types of hallucinations and prevention tactics |

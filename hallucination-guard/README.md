# 🛡️ Hallucination Guard

### Reasoning Integrity & Evidence Verification Skill for Claude

---

## What Is This?

**Hallucination Guard** is a Claude skill that transforms how Claude handles
uncertainty. Instead of just trying not to hallucinate, it runs a structured
**8-stage verification pipeline** on every answer before it reaches you — actively
hunting for unsupported claims, hidden assumptions, missing evidence, and
contradictions.

The result is Claude behaving less like a confident autocomplete and more like
a careful analyst: every claim is traced to a source, assumptions are surfaced
explicitly, missing information is flagged rather than invented, and uncertainty
is communicated instead of hidden.

---

## Why This Exists

LLMs hallucinate because they're optimised to produce fluent, helpful-sounding
completions — even when the underlying evidence doesn't exist. The standard advice
("don't hallucinate") doesn't work because it puts no structural pressure on the
generation process.

This skill puts that pressure in at the workflow level:

| Without this skill | With this skill |
|--------------------|-----------------|
| Invents specific statistics | Flags unsourced numbers |
| Assumes your tech stack | Lists assumptions explicitly |
| States predictions as fact | Labels speculation 🔴 |
| Ignores prior contradictions | Detects and resolves them |
| Fills gaps with plausible-sounding answers | Surfaces gaps and asks |
| One confident explanation | Three alternatives before committing |

---

## The 8-Stage Pipeline

Every query runs through this pipeline. Stages that don't apply (e.g. Alternative
Explanation Generator for simple factual lookups) are skipped automatically.

```
User Query
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 1 │ Fabrication Risk Assessment   │
│         │ Assigns Low / Medium / High   │
│         │ risk tier to the query domain │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 2 │ Evidence Audit                │
│         │ Traces every significant claim│
│         │ to a source. Removes invented │
│         │ details.                      │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 3 │ Assumption Scanner            │
│         │ Lists hidden assumptions.     │
│         │ Converts facts-about-user     │
│         │ into conditional statements.  │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 4 │ Missing Information Check     │
│         │ Identifies what's needed for  │
│         │ a reliable answer. Surfaces   │
│         │ gaps instead of filling them. │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 5 │ Contradiction Detector        │
│         │ Compares draft against prior  │
│         │ messages and constraints.     │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 6 │ Alternative Explanation Gen.  │
│         │ (skipped for simple lookups)  │
│         │ Generates ≥3 alternatives     │
│         │ before committing to one.     │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 7 │ Confidence Scoring            │
│         │ ✅ Verified  95–100%          │
│         │ 🟢 Supported 80–94%           │
│         │ 🟡 Plausible 50–79%           │
│         │ 🔴 Speculative 0–49%          │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│ Stage 8 │ Final Response                │
│         │ Annotated answer with         │
│         │ assumptions, confidence,      │
│         │ gaps, and resolved conflicts. │
└─────────────────────────────────────────┘
```

---

## Claim Classification System

When useful, responses use this four-class taxonomy:

```
VERIFIED           — Directly evidenced by user input or verifiable knowledge
SUPPORTED INFERENCE — Reasonable deduction from available evidence
SPECULATION        — Possible but lacking meaningful support
UNKNOWN            — No evidence available; not guessable
```

**Example — "When will GPT-6 be released?"**

```
VERIFIED:
  OpenAI has publicly released GPT-4 and GPT-4o variants.

SUPPORTED INFERENCE:
  Future model development is ongoing; a successor is plausible.

SPECULATION:
  GPT-6 could arrive within the next 1–3 years based on historical cadence.

UNKNOWN:
  No public release timeline exists for GPT-6.
```

---

## Fabrication Risk Tiers

Different query types carry different hallucination risk. The pipeline assigns
a risk tier at Stage 1:

| Tier | Domains | Extra Precautions |
|------|---------|-------------------|
| 🟢 Low | Math, programming syntax, established science | Standard pipeline |
| 🟡 Medium | Business advice, tech recommendations, product comparisons | Explicit assumptions required |
| 🔴 High | Legal, medical, financial, news, company data, future events | Full pipeline + ⚠️ High-Risk Domain notice |

---

## The Unknown Recognition Engine

The most important component of this skill.

> **Saying "I don't know" is always preferred over guessing.**

Most AI hallucinations come from the instinct to be helpful even when evidence
is absent. This skill actively trains against that instinct:

- Gaps in information are **surfaced**, not filled
- Speculation is **labelled**, not presented as analysis
- Missing data is **listed as requirements**, not substituted with assumptions

---

## When Does This Skill Trigger?

The skill triggers on:

- Explicit requests: *"verify this"*, *"fact-check"*, *"how confident are you"*,
  *"don't hallucinate"*, *"check your sources"*
- Analysis requests: *"analyse this"*, *"review this answer"*, *"investigate"*
- High-risk domain queries: legal, medical, financial, news, company information,
  future predictions — even without an explicit verification request
- Debugging and architecture sessions where claim accuracy matters
- Any time the user provides context and wants a trustworthy synthesis

---

## File Structure

```
hallucination-guard/
├── SKILL.md                              ← Main pipeline instructions
├── README.md                             ← This file
└── references/
    ├── evidence-audit.md                 ← Stage 2 worked examples & edge cases
    ├── missing-info-patterns.md          ← Stage 4 gap patterns by domain
    ├── confidence-calibration.md         ← Stage 7 scoring guidance
    └── hallucination-taxonomy.md         ← 8 hallucination types + prevention
```

### Reference Files

| File | Purpose | Read when |
|------|---------|-----------|
| `evidence-audit.md` | Worked examples of the claim→source→verdict flow | Auditing a tricky claim |
| `missing-info-patterns.md` | Domain-specific lists of commonly missing info | Stage 4, for debugging / legal / medical etc. |
| `confidence-calibration.md` | Calibration guide to avoid over/underconfidence | Scoring claims |
| `hallucination-taxonomy.md` | Eight hallucination types with diagnostic questions | Reviewing a full draft |

---

## Conceptual Foundations

This skill is grounded in several established ideas:

**Epistemic Humility** — The philosophical tradition of recognising the limits of
one's knowledge. Applied here: Claude should know what it doesn't know, and say so.

**Evidence-Based Reasoning** — Every claim requires a warrant (evidence that
supports it) and backing (the source of that evidence). Toulmin's model of
argumentation, applied to AI output.

**Calibrated Uncertainty** — From forecasting research (Tetlock's superforecasters):
accurate confidence estimates are more valuable than confident-sounding ones.
Being right about your uncertainty is as important as being right about the facts.

**Adversarial Self-Review** — Before publishing, an analyst asks: "What's wrong
with this?" This skill builds that adversarial review into the generation process.

**The UNKNOWN Class** — Inspired by Donald Rumsfeld's known unknowns / unknown
unknowns framework: explicitly representing "no evidence available" as a first-class
response category, not a failure state.

---

## Limitations

- **This skill does not give Claude internet access.** It cannot verify claims
  against live sources. It can only trace claims to what the user provided or
  what exists in Claude's training knowledge.
- **Confidence scores are qualitative**, not statistically calibrated. Use them
  as communication tools, not precise measurements.
- **High-Risk domain notices are informational**, not a substitute for a qualified
  professional in legal, medical, or financial matters.
- **The pipeline adds reasoning overhead.** For simple, low-stakes queries, a
  standard response without the full pipeline is often more efficient.

---

## Installation

Install this `.skill` file via your Claude skill manager. Once installed, the
skill will appear in Claude's `available_skills` list and trigger automatically
on matching queries.

---

*Built with the Hallucination Guard skill framework.*
*Skill format: `.skill` (YAML frontmatter + Markdown body)*

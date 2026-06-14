---
name: fine-tuning-simulator
description: >
  Simulate a fine-tuned AI model without actual fine-tuning costs or infrastructure. Use this skill whenever
  the user wants to create a specialized AI persona, build a domain-specific assistant, test what a fine-tuned
  model might behave like, enforce consistent brand voice, or configure Claude to act as a custom AI product.
  Trigger on phrases like "fine-tune Claude for my use case", "make Claude act like a [domain] expert",
  "I want a custom AI assistant for my team", "simulate a fine-tuned model", "create a persona for Claude",
  "I want Claude to always respond like a [role]", "soft fine-tuning", "build me a custom model config",
  "make Claude sound like our brand", or any request to specialize Claude's behaviour for a specific domain,
  company, or use case. Always use this skill — even for simple persona requests — it produces a reusable
  config artifact that the user can save, reload, and iterate on.
---

# Fine-Tuning Simulator

Actual fine-tuning is expensive (thousands of dollars, weeks of compute, MLOps overhead). This skill
reverse-engineers what fine-tuning *actually does* — and achieves it through structured prompting that
persists across the session and produces a portable, reusable config file.

## What Fine-Tuning Really Does (and How We Replicate It)

| Fine-Tuning Layer | This Skill's Equivalent |
|---|---|
| Persona / role identity | `[PERSONA]` block — voice, tone, background, style |
| Domain knowledge injection | `[KNOWLEDGE]` block — facts, terminology, rules-of-thumb |
| Behavioral constraints | `[RULES]` block — hard rules, soft preferences, guardrails |
| Few-shot examples | `[EXAMPLES]` block — positive and negative demonstrations |
| Output format enforcement | `[FORMAT]` block — structure, length, markdown, tone calibration |
| Evaluation / regression tests | `[EVALS]` block — prompts + expected behaviour for consistency checks |

---

## Pipeline

### Phase 1 — Elicit the Config

Interview the user to extract all six layers. Go in order but skip any the user doesn't need.
Do NOT write the config until the interview is complete or the user says "that's enough, build it".

Ask these questions (adapt to what the user has already provided):

**1. PERSONA**
- What role / job title should this model have?
- What's the name (if any) of this AI assistant?
- Describe the voice: formal/casual? warm/clinical? verbose/terse?
- What's the backstory or professional background?
- Who is the target user? (affects how the model explains things)

**2. KNOWLEDGE**
- What domain(s) does this model specialise in?
- Are there proprietary terms, acronyms, or internal jargon?
- What facts, heuristics, or rules-of-thumb must the model always know?
- Any documents, manuals, or reference material to ingest? (user can paste or upload)

**3. RULES**
- Hard rules: what must the model NEVER do or say?
- Soft rules: what should it prefer or avoid when possible?
- Regulatory/compliance constraints (HIPAA, GDPR, financial disclaimers, etc.)?
- Escalation logic: when should it say "contact a human"?

**4. EXAMPLES (Few-Shot)**
- Can the user provide 2–5 example Q&A pairs showing ideal behaviour?
- Can they provide 1–2 negative examples (wrong behaviour to avoid)?
- If no examples provided, generate plausible ones and ask for confirmation.

**5. FORMAT**
- How long should responses typically be?
- Should it use markdown? bullet points? numbered steps?
- Does it always open or close with something specific?
- Any temperature-like calibration: creative vs. precise?

**6. EVALS (optional but recommended)**
- Ask for 3–5 test prompts the model should handle correctly.
- These become the regression suite for consistency checking.

---

### Phase 2 — Generate the FT-Config

Once interview is complete, output the full config in this exact format:

```
════════════════════════════════════════════════════════════
  FINE-TUNING SIMULATOR CONFIG
  Model Name: [name]
  Domain:     [domain]
  Version:    1.0
  Created:    [date]
════════════════════════════════════════════════════════════

[PERSONA]
Name: ...
Role: ...
Voice: ...
Background: ...
Target User: ...

[KNOWLEDGE]
Domain: ...
Key Terms:
  - [term]: [definition]
  ...
Core Heuristics:
  - ...
Reference Facts:
  - ...

[RULES]
Hard Rules (NEVER do):
  ❌ ...
Soft Rules (PREFER / AVOID):
  ⚠️  ...
Compliance:
  - ...
Escalation:
  - ...

[EXAMPLES]
✅ Positive Examples:
  Q: ...
  A: ...

  Q: ...
  A: ...

❌ Negative Examples:
  Q: ...
  Wrong: ...
  Why wrong: ...

[FORMAT]
Response Length: ...
Markdown: yes/no
Structure: ...
Opening: ...
Closing: ...
Calibration: ...

[EVALS]
Test 1: ...
Test 2: ...
Test 3: ...

════════════════════════════════════════════════════════════
```

After generating the config, save it to a `.ft-config` file so the user can download and reload it.

---

### Phase 3 — Activate the Simulated Model

After outputting the config, immediately activate it with this exact declaration:

> **🟢 Simulated Model Activated: [Model Name]**
> This session is now running as [Model Name]. All subsequent responses will follow the config above.
> Type `/ft-status` to review active config. Type `/ft-eval` to run consistency checks. Type `/ft-reload [paste config]` to switch configs.

From this point forward, Claude **becomes** the simulated model. Every response must:
- Stay fully in persona
- Apply all rules without narrating them
- Match the format spec
- Use the knowledge corpus
- Mirror the example style

---

### Phase 4 — Consistency Evaluation (on `/ft-eval` command)

When user types `/ft-eval`, run all `[EVALS]` test prompts through the active config and grade each on:

| Criteria | Score |
|---|---|
| Persona consistency | /10 |
| Rule adherence | /10 |
| Knowledge accuracy | /10 |
| Format compliance | /10 |
| Example style match | /10 |

Output a grade card and flag any failures. Suggest specific config edits to fix failures.

---

### Phase 5 — Iteration Commands

| Command | Action |
|---|---|
| `/ft-status` | Show the full active config |
| `/ft-eval` | Run consistency evaluation suite |
| `/ft-tweak [layer] [change]` | Modify one layer of the config |
| `/ft-add-example` | Add a new few-shot example |
| `/ft-add-rule [rule]` | Add a new hard or soft rule |
| `/ft-export` | Re-output the full config for saving |
| `/ft-reset` | Deactivate and return to standard Claude |
| `/ft-reload` | Load a saved config from paste |
| `/ft-compare` | Run the same prompt twice — as base Claude and as the sim model |

---

## Reference Files

Read when needed:

- `references/domain-knowledge-patterns.md` — Templates for structuring knowledge blocks across 15 common domains (medical, legal, finance, SaaS, e-commerce, etc.)
- `references/persona-archetypes.md` — 20 pre-built persona archetypes with voice calibrations the user can use as starting points or mix

---

## Quality Principles

- **Specificity over generality**: A config that says "be helpful" is worthless. Push for concrete, testable rules.
- **Examples are everything**: The few-shot block is the highest-leverage part of the config. Get real examples if at all possible.
- **Eval-driven iteration**: If the user skips evals, gently push back. Without them, there's no way to know if the config is working.
- **Save the config**: Always output to a downloadable file. The config is the artifact — not just the session behaviour.
- **Negative examples are underrated**: One example of *wrong* behaviour teaches more than three positive ones.

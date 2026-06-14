# 🧠 Fine-Tuning Simulator

> **Soft fine-tuning through structured prompting — without the compute bill.**

Real fine-tuning costs thousands of dollars, weeks of infrastructure time, and ongoing MLOps overhead. This Claude skill reverse-engineers what fine-tuning actually does and delivers it as a structured, reusable session config.

---

## 🎯 What Problem This Solves

Fine-tuning a model does six things:
1. Injects a persistent persona and voice
2. Encodes domain-specific knowledge and terminology
3. Enforces behavioural rules and constraints
4. Teaches through few-shot examples (positive + negative)
5. Controls output format and length
6. Creates a regression surface for consistency testing

This skill replicates all six layers — through structured prompting that persists across the session and produces a portable `.ft-config` file you can save, reload, and iterate on.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎭 Persona Builder | Name, voice, background, target user — fully customisable |
| 📚 Knowledge Injection | Domain terms, heuristics, reference facts |
| 📏 Rule Enforcement | Hard rules (never), soft rules (prefer/avoid), compliance, escalation |
| 💡 Few-Shot Examples | Positive and negative demonstrations of ideal behaviour |
| 📐 Format Control | Length, markdown, structure, opening/closing, tone calibration |
| 🧪 Eval Suite | Built-in consistency checks with scoring |
| 💾 Reusable Config | `.ft-config` file you can save and reload across sessions |
| 🔄 Iteration Commands | `/ft-tweak`, `/ft-add-rule`, `/ft-compare` and more |

---

## 🚀 Getting Started

1. Install the skill in Claude.ai
2. Say: *"Build me a fine-tuned model for [your domain]"*
3. The skill interviews you across 6 layers
4. Your custom model activates in the same session
5. Download your `.ft-config` to reload it any time

---

## 🛠️ Session Commands

Once your simulated model is active, use these commands:

| Command | Action |
|---|---|
| `/ft-status` | Show the full active config |
| `/ft-eval` | Run the consistency evaluation suite |
| `/ft-tweak [layer] [change]` | Modify one layer without rebuilding |
| `/ft-add-example` | Add a new few-shot example |
| `/ft-add-rule [rule]` | Add a hard or soft rule |
| `/ft-export` | Re-output the config for saving |
| `/ft-compare` | Same prompt: base Claude vs. your sim model |
| `/ft-reload` | Load a saved `.ft-config` |
| `/ft-reset` | Return to standard Claude |

---

## 📊 Example Output

```
════════════════════════════════════════════════════════════
  FINE-TUNING SIMULATOR CONFIG
  Model Name: Aria — SaaS Support Specialist
  Domain:     B2B SaaS / Enterprise Software
  Version:    1.0
════════════════════════════════════════════════════════════

[PERSONA]
Name: Aria
Role: Senior Customer Success Engineer
Voice: Warm, precise, and solution-first. Technical depth without condescension.
Background: 8 years in B2B SaaS support. Has handled enterprise escalations
            and product feedback loops.
Target User: Technical users and non-technical buyers at enterprise accounts.

[RULES]
Hard Rules:
  ❌ Never promise a feature or timeline not confirmed in the roadmap
  ❌ Never share internal pricing tiers or discounting authority
Soft Rules:
  ⚠️  Prefer "here's how to accomplish that" over "that's not supported"
  ⚠️  Always check account tier before answering feature questions

[EVALS]
Test 1: "Does your product support SSO?"
Test 2: "When will you support Salesforce integration?"
Test 3: "I'm getting a 429 error on the API"
════════════════════════════════════════════════════════════

🟢 Simulated Model Activated: Aria
```

---

## 📁 Use Cases

- **SaaS companies** — Build a consistent support or sales persona across the team
- **Agencies** — Encode client brand voice into a reusable config
- **Educators** — Create subject-specific tutors with controlled pedagogy
- **Pre-fine-tuning testing** — Prototype the behaviour before spending on real fine-tuning
- **Compliance teams** — Build an assistant that enforces regulatory rules
- **Founders** — Create a domain expert for your specific industry context

---

## 📚 Included References

| File | Contents |
|---|---|
| `references/domain-knowledge-patterns.md` | 15 vertical templates (medical, legal, SaaS, finance, HR, etc.) |
| `references/persona-archetypes.md` | 20 pre-built personas with voice calibration (consultant, staff engineer, tutor, etc.) |

---

## 📦 Installation

1. Download `fine-tuning-simulator.skill`
2. Open Claude.ai → Settings → Skills
3. Upload the `.skill` file
4. Start a conversation: *"I want to build a custom AI assistant for [domain]"*

---

## 💡 Tips

- **Examples are everything** — The few-shot block is the highest-leverage part of your config. Provide real Q&A pairs if you can.
- **Negative examples work** — One example of wrong behaviour teaches more than three positive ones.
- **Run `/ft-eval` early** — Catch config gaps before they compound.
- **Save your `.ft-config`** — The config is the artifact. Export and store it so you can reload without rebuilding.
- **Use `/ft-compare`** — Run the same prompt as base Claude and your sim model to see the delta clearly.

---

*Built for Claude.ai — no developer setup required.*

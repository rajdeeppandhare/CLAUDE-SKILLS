# 📣 Marketing Skills — Claude Skill Portfolio

> A collection of production-ready Claude skills for marketing tasks — copywriting, conversion rate optimisation, cold email, and SEO. Built for founders and marketers who want AI agents to apply real frameworks, not generic advice.

---

## ✨ What's Inside

Four skills covering the core marketing stack — each with routing logic, reference docs, and enough depth to actually be useful:

| Skill | What It Does |
|---|---|
| 📝 `copywriting` | Writes and rewrites marketing copy for any page — homepage, landing page, pricing, feature, about |
| 📈 `cro` | Audits pages and flows for conversion problems — diagnoses the root cause, prioritises fixes by ICE score |
| 📧 `cold-email` | Writes B2B cold email sequences — personalised, short, human, and built to get replies |
| 🔍 `seo-audit` | Audits sites for SEO issues across indexing, technical, on-page, and authority — with keyword strategy built in |

---

## 📦 Installation

### Option 1 — Install the `.skill` file

1. Download the `.skill` file for whichever skill you want
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder

1. Clone or download this repo
2. Copy the skill folder(s) you want into your project's `.agents/skills/` directory
3. Claude Code and compatible agents will detect them automatically

### Option 3 — CLI Install

```bash
npx skills add your-username/marketing-skills
```

---

## 🚀 Usage

Just talk to Claude naturally — skills activate on intent:

```
"Write homepage copy for my B2B SaaS"
→ Uses copywriting skill

"Why isn't my landing page converting?"
→ Uses cro skill

"Write me a 3-email cold outreach sequence for CTOs"
→ Uses cold-email skill

"Audit my site's SEO and tell me what to fix first"
→ Uses seo-audit skill
```

All skills check for a product marketing context file first (`.agents/product-marketing.md`) so they understand your product, audience, and positioning before doing anything.

---

## 🗂️ Skill Details

### 📝 Copywriting

**Triggers:** "write copy for," "improve this copy," "rewrite this page," "headline help," "value proposition," "hero section"

**What it does:**
- Gathers page purpose, audience, offer, and traffic context before writing
- Applies clarity-over-cleverness and benefits-over-features principles
- Structures copy across all page types: homepage, landing page, pricing, feature, about
- Outputs copy with section-by-section annotations and headline alternatives

**References:**
- `copy-frameworks.md` — headline formulas, page templates, section patterns, transition phrases
- `voice-of-customer.md` — VOC research guide, where to find customer language, extraction templates

---

### 📈 CRO

**Triggers:** "improve conversions," "why isn't this converting," "CRO audit," "optimize this page," "fix my signup flow"

**What it does:**
- Audits pages across five lenses: Clarity, Relevance, Friction, Anxiety, Motivation
- Covers all page types: homepage, landing page, pricing, signup forms, popups
- Scores issues by ICE (Impact, Confidence, Ease) and prioritises accordingly
- Recommends A/B tests ordered by expected impact

**References:**
- `conversion-principles.md` — the five conversion levers, benchmarks, A/B test prioritisation
- `cro-patterns.md` — eight proven fix patterns for common conversion problems, microcopy swipe file

---

### 📧 Cold Email

**Triggers:** "cold email," "outreach sequence," "sales email," "follow-up sequence," "reach out to leads"

**What it does:**
- Writes full sequences (3 or 5 email) with subject lines and send cadence
- Applies PPPP, AIDA, Before/After, and Insight Lead formulas
- Structures personalisation across three tiers (1:1 through scaled)
- Includes LinkedIn outreach variants and deliverability checklist

**References:**
- `outreach-playbook.md` — trigger event sequences, personalisation research patterns, industry templates, deliverability checklist
- `email-formulas.md` — formula breakdowns with annotated examples, subject line swipe file, reply rate benchmarks

---

### 🔍 SEO Audit

**Triggers:** "SEO audit," "why isn't my site ranking," "fix my SEO," "keyword research," "on-page SEO," "technical SEO"

**What it does:**
- Audits across four layers: indexing, technical foundation, on-page, authority
- Runs keyword research and intent classification
- Performs competitive gap analysis
- Outputs a structured findings report with 30-day action plan

**References:**
- `seo-checklist.md` — complete 100-point technical and on-page audit checklist with status tracking
- `keyword-strategy.md` — full keyword research process, intent classification, topic clustering, SERP feature targeting

---

## 🗃️ Folder Structure

```
marketing-skills/
├── copywriting/
│   ├── SKILL.md
│   └── references/
│       ├── copy-frameworks.md
│       └── voice-of-customer.md
├── cro/
│   ├── SKILL.md
│   └── references/
│       ├── conversion-principles.md
│       └── cro-patterns.md
├── cold-email/
│   ├── SKILL.md
│   └── references/
│       ├── outreach-playbook.md
│       └── email-formulas.md
└── seo-audit/
    ├── SKILL.md
    └── references/
        ├── seo-checklist.md
        └── keyword-strategy.md
```

---

## 🔗 How Skills Connect

These four skills work best together:

```
seo-audit ──► copywriting ──► cro ──► cold-email
    │               │           │
    │          (writes the   (fixes    (gets the
    │            page)      the page)  right people
    │                                  to the page)
    └──► keyword strategy feeds copywriting topics
```

Example workflow:
1. **seo-audit** — find what keywords to target and what's broken
2. **copywriting** — write or rewrite the page to rank and convert
3. **cro** — audit the page for conversion issues
4. **cold-email** — reach out to high-value prospects while organic builds

---

*Part of a Claude skill portfolio — built alongside [cybersecurity-excavator](https://github.com), [global-accountant](https://github.com), [stock-agent](https://github.com), and [context-memory](https://github.com). Each skill follows a consistent installable format for easy extension and sharing.*

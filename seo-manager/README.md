# 🔍 SEO Manager — Claude Skill

> A comprehensive SEO audit skill for Claude that digs through your pages, HTML, and content to surface every ranking opportunity — with plain English explanations, priority scores, and exact copy-paste fixes.

---

## ✨ What It Does

Paste in a URL, raw HTML, or page content and the SEO Manager will:

- 🕵️ **Audit** every on-page, technical, and content SEO signal
- 🎯 **Score** your page out of 100 with a full breakdown
- 📖 **Explain** why each issue hurts your rankings (no jargon)
- 🔧 **Fix** it — gives you exact corrected code, ready to copy-paste
- 📋 **Prioritise** — tells you what to fix first for maximum impact
- 💡 **Suggest** keyword opportunities you're missing

---

## 🚨 What It Catches

| Severity | Examples |
|---|---|
| 🔴 Critical | Missing title tag, no H1, accidental noindex, broken canonical |
| 🟠 High | Title too long, missing keyword in H1, images without alt text, thin content |
| 🟡 Medium | Broken heading hierarchy, unfriendly URLs, missing Open Graph tags |
| 🟢 Quick Wins | Add FAQ schema, lazy load images, keyword in first 100 words |

**Works for**: Blog posts, landing pages, e-commerce, local business, portfolios, and more.

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `seo-manager.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `seo-manager/` folder
3. Rename to `seo-manager.skill`
4. Install via Claude.ai Settings → Skills

---

## 🚀 How to Use

Once installed, just talk to Claude naturally:

```
"Excavate this page for SEO issues"
"Audit my blog post for SEO"
"Why isn't my page ranking?"
"Check my meta tags"
"How can I improve my SEO score?"
```

Then paste your HTML source or page content — Claude runs the full audit automatically.

---

## 📊 Example Output

```
## 🔍 SEO Excavation Report

Target: /blog/best-running-shoes
Primary Keyword: best running shoes 2024
Overall SEO Score: 61/100
Estimated Ranking Potential: Needs Work → Good (with fixes)

---

### 📊 Score Breakdown
| Category        | Score | Status |
|----------------|-------|--------|
| Technical SEO  | 18/25 | 🟡     |
| On-Page SEO    | 12/25 | 🟠     |
| Content Quality| 20/25 | ✅     |
| UX Signals     | 11/25 | 🟡     |

---

### 🚨 Findings (5 total)

#### 🟠 High — Keyword Missing from H1
What it is: Your H1 says "Top Picks for Runners" — it doesn't contain your target keyword
Why it hurts rankings: Google weighs H1 heavily as a relevance signal
Current: <h1>Top Picks for Runners</h1>
Fix:     <h1>Best Running Shoes 2024: Top Picks for Every Runner</h1>
Impact if fixed: High
...
```

---

## 📁 Skill Structure

```
seo-manager/
├── SKILL.md                        # Core skill logic & audit rules
├── README.md                       # This file
├── seo-manager.skill             # Installable skill package
└── references/
    ├── meta-tags-reference.md      # Complete meta tag formats & formulas
    ├── schema-markup-guide.md      # Structured data for rich results
    └── content-checklist.md        # Content quality & keyword checklist
```

---

## 🧠 Built With

- [Claude Skills](https://claude.ai) — Anthropic's skill system for extending Claude
- [Google Search Central](https://developers.google.com/search) — Official SEO guidelines
- [Schema.org](https://schema.org) — Structured data standards

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built as a portfolio project demonstrating Claude skill development and SEO expertise.*

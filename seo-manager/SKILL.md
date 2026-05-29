---
name: seo-manager
description: >
  Deeply audits web pages, HTML, content, and site structure for SEO issues and opportunities. Use this skill whenever a user wants to: audit a page for SEO, improve search rankings, check meta tags/titles/descriptions, analyze heading structure, review keyword usage, check schema markup, assess content quality for SEO, compare against competitors, get actionable SEO fixes with code examples, or understand why their site isn't ranking. Trigger for any mention of "SEO audit", "improve my rankings", "optimize my page", "why isn't my site ranking", "check my meta tags", "keyword optimization", "search engine optimization", "Google ranking", "organic traffic", "on-page SEO", "technical SEO" — even if the user just says "make my site rank better" or "is my SEO good".
---

# SEO Manager

A comprehensive SEO audit skill that digs through pages, HTML, and content to surface every ranking opportunity — with plain English explanations, priority scores, and exact fixes.

---

## What This Skill Does

Given a URL, raw HTML, page content, or site description, the SEO Manager will:

1. **Excavates** — Scans all on-page, technical, and content SEO signals
2. **Scores** — Rates each finding: 🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Quick Win / ℹ️ Info
3. **Explains** — Why each issue hurts rankings, in plain English
4. **Fixes** — Exact corrected code or content, ready to copy-paste
5. **Prioritises** — Tells you what to fix first for maximum ranking impact
6. **Estimates** — Rough impact each fix will have on organic visibility

---

## Audit Categories

### 🔴 Critical (kills rankings)
- Missing or duplicate `<title>` tag
- Missing `<meta name="description">`
- No H1 tag, or multiple H1s
- Page not indexable (`noindex` set accidentally)
- Broken canonical tag pointing to wrong URL
- Missing or misconfigured robots.txt blocking key pages

### 🟠 High Impact
- Title tag too long (>60 chars) or too short (<30 chars)
- Meta description too long (>160 chars) or missing target keyword
- H1 doesn't contain primary keyword
- Images missing `alt` attributes
- Slow page load signals (large uncompressed images, render-blocking scripts)
- Missing or broken internal links
- No schema/structured data markup
- Thin content (under 300 words for informational pages)
- Keyword stuffing or over-optimisation

### 🟡 Medium Impact
- Heading hierarchy broken (H1 → H3, skipping H2)
- URL not SEO-friendly (too long, dynamic params, no keywords)
- Missing Open Graph / Twitter Card tags
- No sitemap linked
- Duplicate content signals
- Outbound links all `nofollow` unnecessarily
- Missing breadcrumb markup
- Content readability score too low for target audience

### 🟢 Quick Wins (easy + valuable)
- Add FAQ schema to content with questions
- Add `loading="lazy"` to below-fold images
- Add keyword to first 100 words of content
- Compress images without quality loss
- Add internal links to/from high-authority pages
- Improve anchor text on existing internal links

### ℹ️ Info / Best Practice
- Page language attribute (`<html lang="en">`)
- Viewport meta tag present
- Favicon present
- Social sharing tags complete
- Print stylesheet present

---

## Output Format

Always structure the response as:

```
## 🔍 SEO Excavation Report

**Target**: [URL or page name]
**Primary Keyword**: [detected or ask user]
**Overall SEO Score**: [X/100]
**Estimated Ranking Potential**: [Poor / Needs Work / Good / Strong]

---

### 📊 Score Breakdown
| Category | Score | Status |
|---|---|---|
| Technical SEO | X/25 | 🔴/🟠/🟡/✅ |
| On-Page SEO | X/25 | |
| Content Quality | X/25 | |
| User Experience Signals | X/25 | |

---

### 🚨 Findings ([N] total)

#### [SEVERITY EMOJI] [SEVERITY] — [Issue Name]
**What it is**: [plain English, 1 sentence]
**Why it hurts rankings**: [specific Google/search engine impact]
**Current**: [what's there now]
**Fix**:
[exact corrected code or content]
**Impact if fixed**: [Low / Medium / High / Very High]

---

### ✅ What's Already Good
[acknowledge positives]

### 🎯 Priority Fix List (do these first)
1. [Highest impact fix]
2. ...

### 💡 Keyword Opportunities
[Suggested related keywords/phrases to target based on content]

### 📈 Expected Impact
[Overall assessment of ranking improvement if all fixes applied]
```

---

## Analysis Approach

### Given a URL:
- Ask the user to paste the page's HTML source (Ctrl+U in browser) if you can't fetch it
- Or ask them to describe: title, headings, word count, target keyword, current ranking

### Given raw HTML:
- Parse `<head>` first: title, meta description, canonical, robots, OG tags
- Then `<body>`: H1-H6 structure, image alts, internal links, content length, keyword presence
- Check schema markup in `<script type="application/ld+json">`

### Given page content (text only):
- Assess keyword usage, readability, content depth, heading structure
- Flag thin content, keyword gaps, readability issues
- Suggest content improvements with examples

### Given a site description:
- Provide architecture-level SEO recommendations
- Cover: site structure, URL strategy, internal linking strategy, content plan

### Competitor comparison mode:
- User provides their page + competitor URL/content
- Compare: content depth, keyword targeting, heading strategy, schema usage, backlink signals
- Identify gaps and opportunities

---

## Keyword Analysis Guidelines

- **Primary keyword**: should appear in title, H1, first 100 words, meta description, URL
- **Secondary keywords**: naturally throughout content, in H2/H3s where relevant
- **Keyword density**: aim for 1-2% — flag if over 3% (stuffing) or under 0.5% (underoptimised)
- **Semantic keywords**: suggest LSI/related terms that strengthen topical authority
- **Search intent**: always assess — is the content matching informational / navigational / transactional intent?

---

## Content Quality Scoring

Rate content on:
- **Depth**: Does it fully answer the target query?
- **Uniqueness**: Generic vs original insights?
- **Readability**: Flesch score equivalent — short sentences, clear language
- **E-E-A-T signals**: Experience, Expertise, Authoritativeness, Trustworthiness
- **Freshness**: Is the content dated? Needs updating?
- **Engagement hooks**: Headers, lists, images, examples — does it hold attention?

---

## Tone Guidelines

- **Plain English first**: explain what each SEO term means before using it
- **Business impact focused**: connect fixes to traffic, leads, revenue where possible
- **Realistic**: don't promise #1 rankings — talk about improvement potential
- **Actionable**: every finding has a concrete next step
- **Beginner-friendly**: assume user may not be a developer

---

## Edge Cases

- **No code provided** → ask for HTML source or key page details, offer to audit based on description
- **Local business page** → add local SEO checks: NAP consistency, Google Business Profile signals, local schema
- **E-commerce page** → add product schema, review schema, price/availability markup checks
- **Blog/article** → add Article schema, author markup, publish date freshness checks
- **Landing page** → focus on conversion + SEO balance, CTA placement, trust signals
- **Already ranking well** → focus on maintaining + improving, identify next keyword opportunities

---

## Reference Files

- `references/meta-tags-reference.md` — Complete meta tag guide with correct formats (load for technical audits)
- `references/schema-markup-guide.md` — Schema.org markup patterns for common page types (load when checking structured data)
- `references/content-checklist.md` — Content quality and keyword optimisation checklist (load for content audits)

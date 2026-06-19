# Gap Frameworks Reference

Three layers of competitive gap analysis, from obvious to high-signal.

---

## Layer 1 — Surface Gaps

**What it finds:** Feature-level differences between competitors.  
**Value:** Low — this is table-stakes. Every SWOT does this.  
**When to use:** As the baseline scan only. Never stop here.

### Method
Build a feature matrix. For each competitor:
1. Scrape homepage, pricing page, and docs nav
2. List every feature mentioned in their marketing copy
3. Cross-reference against competitors
4. Mark gaps: features present in 1–2 competitors but absent in others

### Common Layer 1 signals
- Missing integration (e.g. Slack, Salesforce)
- Missing platform (mobile, desktop, API)
- Missing workflow (e.g. bulk operations, reporting)
- Pricing tier that doesn't exist (e.g. free tier, enterprise tier)

### Layer 1 failure mode
You output "Competitor A doesn't have X" without checking whether:
- X was tried and removed (graveyard)
- X exists but isn't marketed
- X is on their roadmap (check changelog)
- The absence is intentional positioning, not a gap

---

## Layer 2 — Segment Gaps

**What it finds:** Buyer segments, verticals, or personas competitors systematically ignore.  
**Value:** High — this is where positioning wedges come from.  
**Why competitors miss this:** They copy each other's GTM, not the full market.

### Signal A: Case Study / Testimonial Skew

Pull the "Customers" or "Case Studies" page for each competitor.

Ask:
- What company size dominates? (Enterprise? Mid-market? SMB?)
- What verticals appear? Which are absent?
- What buyer persona is quoted? (CTO? Marketing? Ops?)
- Who is *not* represented? That's your signal.

**Example:** If 80% of case studies are enterprise SaaS companies, the SMB, agency, and solo-operator segments are likely underserved — not because they don't have the problem, but because the GTM never prioritized them.

---

### Signal B: Review Use-Case Mining

Sources: G2, Capterra, Reddit (r/[category], r/entrepreneur, r/smallbusiness), ProductHunt, AppSumo, TrustRadius.

Search: `"[Competitor]" site:reddit.com` or G2/Capterra review pages.

Look for:
- Use cases mentioned in reviews that don't appear in marketing copy
- Workarounds: "I use [Competitor] for X even though it's not designed for it"
- Wish-list items: "I wish [Competitor] did Y" (especially with upvotes/agreement)
- Workflow descriptions that reveal an unaddressed persona

**How to identify a segment signal vs. a feature request:**
A segment signal is a *who*, not a *what*. "Solo founders use this differently than ops teams and the product doesn't accommodate that" is a segment signal. "I wish they had dark mode" is a feature request.

---

### Signal C: Geography / Regulatory Gaps

Check for:
- Pricing currency (USD-only = implicit market exclusion)
- Language support (English-only = LATAM, APAC, EMEA exclusion)
- Compliance coverage: GDPR, HIPAA, SOC2, CCPA — each one excludes segments
- Case studies with geographic distribution — if all US/UK, no DACH, LATAM, SEA investment
- Tax / invoicing support (VAT, GST) — its absence signals intentional market scope

**Why this matters:** Regulatory compliance is often where incumbents won't go because it requires legal overhead. That overhead is a moat for a new entrant willing to build it.

---

### Signal D: Persona Mismatch

The product "technically works" for a persona but was never designed for them.

Red flags:
- Onboarding assumes a technical buyer (API keys, webhooks in step 1)
- UI complexity requires training — implies team / ops context
- Terminology assumes enterprise familiarity (e.g. "workspaces," "roles," "SSO")
- Pricing requires a credit card approval process implying procurement

**Why it matters:** A persona mismatch creates churn and bad reviews — but also a clean wedge. If the incumbent can't simplify without a rewrite, a new entrant can own the simpler persona permanently.

---

## Layer 3 — Negative-Space Gaps

**What it finds:** What competitors are *actively avoiding* — silence, redirection, and patterns of omission.  
**Value:** Highest — these gaps often have structural durability because competitors can't easily fix them.  
**Why competitors miss this:** Nobody looks at what's *not* there.

---

### Scan A: Pricing Page Archaeology

Pricing pages are political documents. What they hide is as informative as what they show.

Look for:
- **"Contact sales" walls at a specific threshold** → That segment is unprofitable or politically awkward to serve at scale
- **No monthly option** → Commitment requirement filters out low-intent or cash-constrained segments
- **No free tier or trial** → High-touch sales motion, excludes self-serve buyers
- **Floor price ≥$X/mo** → Anything below that floor is an intentional gap
- **Usage caps that de facto exclude SMBs** → Often presented as "generous" but actually designed for enterprise consumption levels
- **Missing tier** → If they have $29 and $299 but nothing in between, someone in that range is underserved

---

### Scan B: Changelog / Release Note Analysis

The changelog is the most honest document a company publishes. It shows what they actually built, not what they say they prioritize.

How to find it:
- `[Competitor] changelog`
- `[Competitor] release notes`
- `[Competitor] what's new`
- Check their blog for monthly/quarterly update posts

Look for:
- **Categories absent for 12+ months** → Deprioritized or architecturally hard
- **Features announced but never shipped** → Signals internal difficulty
- **Bug fixes dominating over features** → Engineering capacity consumed by tech debt, less headroom for new segments
- **Community-requested features that never appear** → Check their public roadmap / Canny boards

**Why this matters:** If a competitor hasn't shipped in a category in 12 months, they either can't (architecture) or won't (not their ICP). Both are gaps.

---

### Scan C: Job Posting Signals

What a company hires for = what they're actually building.

Search: `[Competitor] jobs` / `[Competitor] careers` / LinkedIn jobs filtered by company.

Signal mapping:
| What's absent | What it implies |
|---|---|
| No support automation roles | Support is a gap; customers are underserved post-sale |
| No ML/AI engineers | AI features are bolted on (integrations), not core (proprietary) |
| No localization/i18n roles | No international market investment |
| No self-serve / PLG roles | No investment in SMB or bottom-up growth |
| No SMB / mid-market sales roles | Enterprise-only GTM, SMB is unaddressed |
| No DevRel / docs roles | Developer experience is weak; API product is secondary |
| All enterprise AE roles | High-touch sales motion only |

**Hiring lag:** Job postings typically reflect 6–12 month forward plans. Absence of hiring = absence of investment for the next year+.

---

### Scan D: 1–3 Star Review Pattern Mining

1–3 star reviews are pre-validated, uncompensated, real-pain signals. They are the cheapest market research available.

How to mine them:
1. Pull 1–3 star reviews from G2 or Capterra for each competitor
2. Search Reddit: `"[Competitor]" -site:[competitor].com` filtered to recent posts
3. Search AppSumo / ProductHunt comments for negative threads
4. Look at response patterns: what does the company *not* address in their review responses?

Categorize complaints into:
- **UX/usability** → feature request, not a market gap
- **Pricing / value** → potential segment signal
- **Missing workflow** → check if it's a surface or segment gap
- **Support quality** → operational gap (harder to copy)
- **Reliability / performance** → architectural gap (hardest to copy)

**Cross-competitor pattern rule:**
- Pain mentioned in 1 competitor's reviews = that company's execution problem
- Pain mentioned across 2+ competitors' reviews = systemic market gap
- Pain mentioned across 3+ competitors' reviews = structural market failure = highest-value gap

Only systemic gaps qualify for full briefs.

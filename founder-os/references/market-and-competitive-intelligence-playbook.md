# Market & Competitive Intelligence Playbook

Used in Phase 4 of Founder OS. Covers where to look for market and competitive data, how to evaluate source quality, and the template for tracking named competitors. Load this when the user asks for market research, a competitive analysis, or wants to understand the landscape they're operating in.

---

## Table of Contents

1. [Source Strategy](#source-strategy)
2. [Competitive Tracking Template](#competitive-tracking-template)
3. [Market Synthesis Format](#market-synthesis-format)
4. [Research Workflows by Question Type](#research-workflows)
5. [Source Quality Guide](#source-quality-guide)
6. [Anti-Patterns to Avoid](#anti-patterns)

---

## Source Strategy

Different questions need different source types. Match the source to the question — don't default to a Google search for everything.

### For customer pain and market reality

**Reddit / forums** — the highest-signal source for unfiltered customer sentiment. People complain honestly on Reddit in ways they won't on a review site. Search the subreddits where your target customer lives, not just the product's own subreddit.

Search patterns that work:
```
[problem domain] "anyone else" OR "frustrated" OR "wish there was"
[competitor name] "hate" OR "switched" OR "looking for alternative"
[job title / role] "how do you" OR "what do you use for"
```

**Hacker News** — strong signal for technical founders, devtools, B2B SaaS. Use `hn.algolia.com` to search the full archive. Look for "Ask HN: How do you handle X" threads — the top comments are often from practitioners.

**Indie Hackers** — useful for understanding what other founders have tried in adjacent spaces. Revenue numbers are self-reported but directionally useful.

**Product Hunt** — look at the comments on competitor launches, not just the upvotes. The comments surface real user reactions and common complaints.

### For competitive intelligence

**Company websites** — the obvious starting point, but treat pricing pages and feature lists as marketing copy until verified. Look for what they *don't* say as much as what they do.

**G2 / Capterra / Trustpilot reviews** — sort by "most recent" and look at 3-star reviews, not just 5s and 1s. 3-star reviews often contain the most actionable signal: "I like it but wish it did X."

**LinkedIn** — useful for headcount and hiring patterns. A company doubling their engineering team in a category signals investment; a wave of senior departures is a different signal. Job postings reveal product priorities.

**Crunchbase / PitchBook** — funding history, investors, and rough valuation signals. Free tier is sufficient for most early-stage research.

**GitHub** — for developer tools and open-source adjacent products. Star count, contributor activity, and issue volume are all signal. A competitor with a large open-source repo and declining contributor activity is worth noting.

**SEC filings / earnings calls** — for public company competitors. Earnings call transcripts (available on Seeking Alpha) often contain explicit statements about competitive dynamics and market sizing.

### For market sizing

**Bottom-up always beats top-down.** "The TAM is $50B per IDC" is meaningless — it almost always describes a broader category than the one you're actually entering. Build a bottom-up estimate instead:

```
Bottom-up example:
  "There are ~200,000 independent financial advisors in the US.
   We're targeting the 40,000 who use a specific CRM.
   Of those, maybe 20% have the pain point we solve.
   That's 8,000 potential customers.
   At $200/month, that's $19.2M ARR if we captured them all."
```

This is a much smaller number than any analyst report will give you, and it's a much more honest basis for early-stage decisions.

**Primary sources for addressable market data:**
- Bureau of Labor Statistics (US job/industry counts)
- Census Bureau (business counts by NAICS code)
- Trade association reports (often free, highly specific)
- LinkedIn Sales Navigator filters (rough company/person counts by industry/role)

---

## Competitive Tracking Template

Track named competitors with enough specificity to actually be useful. "They're bigger than us" is not useful. "They don't have workflow automation and their G2 reviews mention it in 40% of 3-star reviews" is useful.

```markdown
## Competitive Landscape

**Last updated:** [date]

### [Competitor Name]

- **What they do:** [one sentence — their core use case]
- **Primary customer:** [who they actually serve — be specific]
- **Pricing:** [actual tiers and prices, or "not public" with last-checked date]
- **Funding / size:** [ARR or funding if known, headcount estimate]
- **Key advantage:** [the one thing they do genuinely better than alternatives]
- **Key weakness:** [the most common complaint from real users — cite source]
- **Recent moves:** [any product launches, pivots, or announcements in last 6 months]
- **Threat level:** High / Medium / Low — [one sentence why]
- **Last checked:** [date]

---

### [Competitor Name]

[repeat block]
```

### Competitive positioning summary

After tracking individual competitors, synthesize into a positioning map:

```markdown
### Positioning Map

**Axis 1:** [e.g. Self-serve ←→ High-touch / Enterprise]
**Axis 2:** [e.g. Narrow use case ←→ Platform / All-in-one]

Quadrants:
- Top-right ([label]): [competitors here]
- Top-left ([label]): [competitors here]
- Bottom-right ([label]): [competitors here]
- Bottom-left ([label]): [competitors here]

**Where we sit:** [position] — because [why]
**Open space:** [the quadrant or position with no strong competitor — if there is one]
```

---

## Market Synthesis Format

After researching a market, synthesize rather than listing sources. A wall of links is not intelligence — synthesis is.

```markdown
## Market Synthesis: [Market / Category Name]

**Research date:** [date]
**Sources consulted:** [brief list — Reddit threads, G2 reviews, news, etc.]

### Consensus (things most sources agree on)
- [Point 1]
- [Point 2]

### Disagreements (things sources conflict on)
- [Point] — [Source A says X, Source B says Y — here's why the discrepancy might exist]

### Opportunities (gaps, unmet needs, or underserved segments)
- [Opportunity 1] — [evidence for it]
- [Opportunity 2] — [evidence for it]

### Threats (risks to the opportunity)
- [Threat 1] — [evidence or reasoning]
- [Threat 2]

### Key open questions (things that can't be answered from desk research alone)
- [Question — requires customer conversations / experiments to answer]
```

---

## Research Workflows by Question Type

### "Who are our competitors?"

1. Start with obvious names the founder already knows
2. Search Product Hunt for the category
3. Search G2 for the category — the filters surface named players
4. Search Reddit for "[problem] tool" or "[problem] software" — users name what they use
5. Check who's getting funded in the space (Crunchbase, TechCrunch)
6. Look at the LinkedIn profiles of the target ICP — what tools do they list?

Output: completed competitive tracking table with ≥5 named competitors.

### "How big is this market?"

1. Refuse to cite an analyst TAM figure without a bottom-up sanity check
2. Build the bottom-up model (target company/person count × attach rate × price)
3. Cross-check with any available primary data (BLS, Census, trade association)
4. State the estimate as a range with the key assumptions explicit

Output: bottom-up market size estimate with assumptions visible and a stated confidence level.

### "What do customers actually want?"

1. Search Reddit for the problem domain (3–5 relevant subreddits)
2. Search Hacker News for "Ask HN" threads about the problem space
3. Read G2/Capterra reviews of the top 2–3 competitors — focus on 3-star and the "cons" sections
4. Synthesize the top 3–5 recurring complaints and wish-list items with representative quotes

Output: list of recurring pain points and desired features, cited to real sources, not paraphrased from general knowledge.

### "Why are customers switching away from [Competitor]?"

1. Reddit: search "[competitor] alternative" and "[competitor] switch"
2. G2: filter competitor reviews by "switched from" attribute
3. Product Hunt: read launch comments for migration stories
4. Twitter/X (if relevant): search "[competitor] frustrating" or "[competitor] left"

Output: the top reasons people leave, with evidence, not speculation.

---

## Source Quality Guide

Not all sources are equal. Be explicit about source quality when presenting findings.

| Source type | Signal type | Reliability | Watch out for |
|---|---|---|---|
| Customer Reddit posts | Pain, workarounds, wish-list | High (unfiltered) | Small sample, vocal minority bias |
| G2 / Capterra reviews | Feature gaps, UX issues | Medium-high | Review gating, recency bias |
| Hacker News threads | Technical pain, builder perspective | High for tech | Skewed toward developers |
| Competitor website | Feature list, positioning | Low (marketing) | Aspirational claims |
| Analyst reports (IDC, Gartner) | Broad market size | Low for early-stage | Overly broad categories |
| LinkedIn job postings | Investment signals, priorities | Medium | Lags by 3–6 months |
| Funding announcements | Market validation | Medium | Survivorship bias |
| Founder interviews / podcasts | Strategy and thinking | Medium | Self-serving framing |
| Primary customer interviews | Actual behavior and motivation | Highest | Hard to get, sample size |

### Rules for presenting findings

- **Don't present marketing copy as fact.** If a competitor's website says "the industry-leading platform," that's their claim, not a finding.
- **Cite the source type, not just the conclusion.** "Multiple G2 reviewers mention X" is more useful than "customers find X frustrating."
- **Flag single-source claims.** If something important rests on one source, say so — the founder should weight it accordingly.
- **Separate correlation from causation.** A competitor getting funded doesn't mean the market is validated; it means one set of investors believed it.

---

## Anti-Patterns to Avoid

**❌ The analyst-report market size**
Citing a "$47B market" without a bottom-up sanity check. These numbers almost always describe a category 10–50× larger than the actual addressable segment. Always pair with a bottom-up estimate.

**❌ The competitor list without specificity**
"There are several competitors in this space." Name them. Feature-for-feature comparisons that say "competitor A has more features" without naming the features are useless.

**❌ Recency blindness**
Competitive landscape from 12 months ago is outdated in most fast-moving spaces. Always note when information was last verified, and flag if a competitor entry is more than 3 months old.

**❌ Ignoring indirect competition**
The real competition is often the spreadsheet, the manual process, or the "we just don't do that yet" — not just named software competitors. Ask what the target customer does today *instead* of buying anything.

**❌ Confirming the thesis**
Searching for evidence that the market exists without also searching for evidence that it doesn't, or that it's already served. If you only find confirming evidence, you probably only looked for confirming evidence.

# Credibility Scoring Rubric

Used in Stage 4 of the Research Excavator pipeline. The goal isn't to produce a precise numeric score — it's to force a deliberate check before a source gets to support a claim in the final report.

## The four checks

### 1. Primary vs. secondary

- **Primary**: the original filing, dataset, paper, transcript, legal document, or first-party announcement.
- **Secondary**: someone summarizing, reporting on, or interpreting a primary source.

Secondary sources aren't disqualifying — good journalism and analysis add real value — but when a secondary source makes a specific quantitative claim, prefer tracing it back to the primary source if one is findable and citing that instead.

### 2. Recency relevance

Not "is this old" but "is this *still true*." A five-year-old architectural deep-dive on a stable algorithm can be more relevant than a five-day-old blog post repeating outdated numbers. Ask: does this topic change quickly (pricing, leadership, regulations, software versions) or slowly (historical events, established science, stable processes)? Weight recency accordingly.

### 3. Institutional bias risk

Does the source have a stake in the conclusion it's reporting? Examples:
- A vendor's own benchmark of its product vs. a competitor's
- An advocacy group's statistics on a policy it supports or opposes
- A company's press release about its own performance
- A paid sponsor's "review"

Bias risk doesn't mean automatic exclusion — it means the claim needs corroboration from an independent source before it goes in the "well-supported" bucket, and if it can't get that corroboration, it belongs in "contested or uncertain" with the bias risk noted.

### 4. Corroboration

Has anything independent confirmed this? A single source making an extraordinary, surprising, or high-stakes claim should not anchor a major conclusion in the report. Two or more independent sources agreeing is a meaningfully different evidentiary situation than one source repeated by aggregators (note: aggregators citing the same original source are not independent corroboration — trace back to see if it's actually one source wearing several hats).

## What to do with low scorers

Sources that fail two or more checks (e.g., a biased, uncorroborated, secondary source on a fast-changing topic):
- Don't use them to anchor a "well-supported" claim
- If there's nothing better, you can still mention the claim, but flag it explicitly as low-confidence and say why
- Forums, comment sections, and SEO content farms that visibly exist to rank rather than inform should generally be skipped entirely unless the question is specifically about public sentiment or community discussion (in which case that's the actual primary source for that question)

## Domain-specific notes

**Technical/developer topics**: official docs and changelogs > GitHub issues/PRs from maintainers > Stack Overflow accepted answers > blog posts > unverified tutorials. Version numbers matter — check the doc matches the version in question.

**Financial/market topics**: primary filings (10-K, prospectuses, central bank releases) > analyst notes (note the analyst's firm has a position) > financial news > aggregator summaries > forum sentiment (treat forum sentiment as a "what people think" data point, not a "what is true" data point).

**Health/medical topics**: peer-reviewed studies and clinical guidelines from recognized bodies > major health institution guidance > health journalism citing named experts > general wellness media > forums/anecdote. Be explicit about sample sizes and whether something is preliminary research vs. established consensus.

**Policy/legal topics**: the actual legislation, regulation, or court ruling > government agency guidance > legal analysis from named practitioners > advocacy group framing (from any side) > general news commentary. Policy questions are especially prone to source bias — actively seek out sources with different institutional leanings and note where they diverge.

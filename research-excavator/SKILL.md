---
name: research-excavator
description: Turns a research question into a structured, source-verified report instead of a quick three-link summary. Runs an 8-stage pipeline (scope classification, sub-question planning, parallel evidence gathering, source credibility scoring, cross-source contradiction checking, gap-filling refinement, themed synthesis, and a final citation audit) that explicitly surfaces contested or unverified claims rather than silently picking a side. Use this skill whenever a question needs more rigor than a single web search — comparative questions ("X vs Y"), "what's the current state of...", industry or market overviews, decision-support questions ("should I..."), fact-checking a claim, or any request that asks for sources, citations, or verification. Also trigger on phrases like "research this for me", "give me a deep dive on", "what does the evidence actually say about", "compare these with sources", or "is this true" — even if the user never says the words "report" or "structured."
---

# Research Excavator

A staged research pipeline that replaces single-pass "search and summarize" with decomposition, credibility scoring, and explicit contradiction-surfacing. The point of this skill isn't to run more searches — it's to run the *right* searches and refuse to flatten disagreement between sources into false confidence.

## Why a staged pipeline instead of just searching well

Two failure modes show up constantly in unstructured research: shallow synthesis (jumping from three search snippets straight to a confident summary, never checking whether anything contradicts) and search thrashing (running many near-identical queries without a plan, burning calls without adding coverage). Stages 1–2 below exist to prevent the second. Stages 4–6 exist to prevent the first. Treat the stages as checkpoints to actually pass through, not decoration — the value of this skill over freelancing is that it forces the verification step that's easiest to skip under time pressure.

This skill is for *inline* rigor — research that deserves more than a casual answer but doesn't need a dedicated multi-day investigation. If the user has access to a dedicated deep-research mode and the question genuinely needs exhaustive, many-hour investigation, this skill is still fine to run, but say so plainly in the output (see the scope note in Stage 7) rather than silently pretending this pipeline and a dedicated research mode are the same thing.

## Stage 1 — Scope & Intent Classification

Before searching anything, classify the question into one of four types, because the right amount of research is different for each and guessing wrong is the single biggest cause of wasted effort:

- **Factual lookup** — a single verifiable fact or short list of facts ("what's the current inflation rate in Japan")
- **Comparative analysis** — weighing two or more named options against each other ("Postgres vs DynamoDB for this use case")
- **Open-ended exploration** — mapping a space with no single right answer ("state of the AI coding agent market")
- **Decision support** — the user needs to *do* something with the answer ("should I refinance now")

Also pin down implicit constraints the user didn't state outright: timeframe (do they want current state or historical context?), geography, depth (a paragraph or a full report?), and audience (technical or general). Write these down explicitly before moving on — they shape every later stage.

## Stage 2 — Research Planning

Decompose the question into 3–7 sub-questions or angles, sized to the Stage 1 classification:

| Classification | Typical angles | Sources per angle | Rough call budget |
|---|---|---|---|
| Factual lookup | 1–2 | 1–2 | 2–4 calls |
| Comparative analysis | 3–5 (often one per option, plus shared criteria) | 2–3 | 8–15 calls |
| Open-ended exploration | 4–6 | 2–3 | 12–20 calls |
| Decision support | 3–5 | 2–4 | 10–18 calls |

These are starting points, not contracts — let the actual complexity of what you find adjust the plan in Stage 6. The budget column matters: if you're tracking toward 20+ calls, that's not a reason to stop, but it is something to mention to the user in the final output (Stage 7) so they know how much ground was actually covered and can decide if they want something more exhaustive.

Write the sub-questions down before searching. Each one becomes a tag you carry through Stage 3 so results don't get jumbled together later.

## Stage 3 — Parallel Evidence Gathering

Search per angle, tagging each result with the sub-question it's answering. Two rules that prevent the most common waste here:

- **No near-identical re-queries.** If a query returns thin results, change the angle of attack (different terms, narrower or broader scope, a different source type) rather than re-running a barely-different version of the same search.
- **Fetch, don't just snippet-read.** Search snippets are frequently too thin to support a real claim. When a result looks load-bearing for the final answer, use web_fetch to read the actual page rather than relying on the search snippet alone.

## Stage 4 — Source Triage & Credibility Scoring

Before using any source to support a claim, score it. See `references/credibility-scoring-rubric.md` for the full rubric and domain-specific heuristics (technical, financial, health, policy), but at minimum check:

- **Primary vs. secondary** — original data/filing/document vs. someone reporting on it
- **Recency relevance** — is this still current, or describing a state that's since changed
- **Institutional bias risk** — does the source have a stake in the conclusion
- **Corroboration** — has anything else independently confirmed this

Sources that fail multiple checks (anonymous forum claims, SEO content farms repeating each other, single-source claims on a contested topic) don't get silently dropped — flag them as low-confidence if you use them at all, and prefer dropping them in favor of better sources when one exists.

## Stage 5 — Cross-Source Verification

This is the stage most research output skips, and it's the one that actually earns trust. Specifically look for places where sources disagree — on a number, a date, a causal claim, a recommendation — and do not silently pick one and move on. When you find a contradiction:

1. State both positions and what each is based on
2. Note which source is more credible per Stage 4, if that's clear
3. If it's genuinely unresolved, say so outright rather than manufacturing false certainty

See `references/contradiction-resolution-patterns.md` for common contradiction types (stale-vs-current data, methodology differences, regional variation, framing/ideology, single unverified claim) and how to present each one without either flattening the disagreement or making it sound like more chaos than it is.

## Stage 6 — Gap Detection & Iterative Refinement

After the first pass, look at what's still thin or unanswered against the Stage 2 sub-questions. Run a second, *targeted* round of searches only on the actual gaps — not a full re-run of everything. This is also the point to revisit the Stage 2 budget: if the topic turned out simpler than expected, stop early; if it turned out more contested or complex, it's fine to go past the original estimate. There's no hard ceiling on calls here — just keep the user informed in Stage 7 about how much ground was actually covered.

## Stage 7 — Synthesis & Structuring

Build the actual report. Organize by theme, not by source — nobody wants "here's what Source A said, here's what Source B said"; they want the question answered with sources backing each point. Lead with the most decision-relevant finding, not the most interesting one. Use this structure:

```markdown
# [Question, restated]

## Bottom line
[2-4 sentences: the actual answer, hedged appropriately if warranted]

## What the evidence supports
[Findings with high source agreement — organized by theme/sub-question, citations inline]

## Contested or uncertain
[Anything from Stage 5 where sources disagreed or evidence was thin — stated plainly, not buried]

## Research scope note
[1-2 sentences: how many angles/sources were covered, and — only if call volume was high (think 20+) or the topic genuinely warrants a more exhaustive investigation — a plain note that a dedicated deep-research mode could go further]

## Sources
[Full list, tagged by which sub-question(s) they supported]
```

Adapt section names to fit a factual-lookup question (which might just need "Bottom line" and "Sources") versus a full comparative or decision-support report (which uses the full template).

## Stage 8 — Citation Audit & Hallucination Check

Final pass before sending the output. For every claim in the report, trace it back to a specific retrieved source — not a plausible-sounding inference you generated while writing. See `references/citation-audit-checklist.md` for the full pass. If a claim can't be traced to something actually retrieved in Stage 3 or 6, either cut it or go back and verify it; don't let a confident-sounding sentence survive on vibes alone.

## Reference files

- `references/credibility-scoring-rubric.md` — full scoring rubric plus domain-specific source weighting (technical, financial, health, policy/legal)
- `references/query-decomposition-playbook.md` — worked examples of breaking down each Stage 1 classification into sub-questions, with call-budget guidance
- `references/contradiction-resolution-patterns.md` — taxonomy of disagreement types and how to present each in Stage 5
- `references/citation-audit-checklist.md` — the Stage 8 pass, item by item
- `references/source-type-weighting-guide.md` — which source types to prioritize per domain (dev/technical, market/finance, health, policy)

Read the relevant one when you hit that stage — don't load all of them upfront for a simple factual-lookup question.

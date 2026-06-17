# Research Excavator

A Claude skill that turns research questions into structured, source-verified reports — instead of a quick three-link summary that quietly picks a side whenever sources disagree.

## The problem this solves

Unstructured "let me search and summarize" research has two predictable failure modes:

**Shallow synthesis.** Claude reads three search snippets and writes a confident paragraph, never checking whether anything else contradicts those snippets, never tracing a paraphrase back to what the source actually said, never distinguishing a well-established fact from a single unconfirmed claim.

**Search thrashing.** Claude runs query after query with no plan, often re-running near-identical searches that don't add coverage, burning effort without actually getting closer to a defensible answer.

Research Excavator fixes both by forcing an 8-stage pipeline instead of letting Claude freelance straight from question to answer.

## The 8-stage pipeline

| Stage | What it does | Why it exists |
|---|---|---|
| 1. Scope & Intent Classification | Classifies the question as factual lookup, comparative analysis, open-ended exploration, or decision support; pins down implicit constraints (timeframe, geography, depth, audience) | Wrong scope = wasted effort in every later stage |
| 2. Research Planning | Decomposes into 3–7 sub-questions, sized to the classification, with a rough call budget | Prevents both over-researching simple questions and under-researching complex ones |
| 3. Parallel Evidence Gathering | Searches per sub-question, tagged so results don't get jumbled; fetches full pages instead of relying on thin snippets | A plan only works if execution actually follows it |
| 4. Source Triage & Credibility Scoring | Scores every source on primary/secondary, recency, institutional bias risk, corroboration | Stops a single biased or unconfirmed source from anchoring a confident claim |
| 5. Cross-Source Verification | Explicitly hunts for contradictions between sources instead of silently picking one | The single most commonly skipped step in AI-generated research — and the one that actually earns trust |
| 6. Gap Detection & Refinement | Targeted second-pass searches only on what's still thin, against the original sub-questions | Avoids both stopping too early and blindly re-running everything |
| 7. Synthesis & Structuring | Builds the final report: bottom line, what's well-supported, what's contested, scope note, sources | Organizes by theme/answer, not by "here's what each source said" |
| 8. Citation Audit & Hallucination Check | Traces every claim in the draft back to something actually retrieved | Catches paraphrase drift, scope-stretching, and claims that survived from sources that got cut earlier |

Stages 1–2 prevent search thrashing. Stages 4–6 prevent shallow synthesis. Stage 8 is the safety net that catches whatever slipped through anyway.

## When this triggers

Built to fire on:
- Comparative questions ("Postgres vs DynamoDB for X")
- "What's the current state of..." / industry or market overview questions
- Decision-support questions ("should I refinance now")
- Explicit fact-checking requests ("is this true")
- Any request that asks for sources, citations, or verification
- Phrases like "research this for me," "give me a deep dive on," "what does the evidence actually say about," "compare these with sources"

It deliberately does **not** fire on simple factual lookups that don't need the full machinery — Stage 1 classifies those as "factual lookup" and the pipeline scales itself down (2–4 calls, mostly "Bottom line" + "Sources," skipping the heavier stages) rather than overproducing a five-section report for a one-fact question.

## File structure

```
research-excavator/
├── SKILL.md                                  # Main pipeline definition (all 8 stages)
├── README.md                                 # This file
└── references/
    ├── credibility-scoring-rubric.md         # Stage 4 — scoring + domain-specific source priority
    ├── query-decomposition-playbook.md       # Stage 2 — worked examples per classification type
    ├── contradiction-resolution-patterns.md  # Stage 5 — taxonomy of why sources disagree
    ├── source-type-weighting-guide.md        # Stage 4 — where to look, by research domain
    └── citation-audit-checklist.md           # Stage 8 — the final claim-by-claim trace pass
```

Reference files are loaded on demand at the relevant stage — not all upfront. A simple factual-lookup question shouldn't pull in all five reference docs just to answer one fact.

## Output format

The default report structure (adapted down for simpler questions):

```markdown
# [Question, restated]

## Bottom line
[2-4 sentences: the actual answer, hedged appropriately if warranted]

## What the evidence supports
[High-agreement findings, organized by theme, citations inline]

## Contested or uncertain
[Anything where sources disagreed or evidence was thin — stated plainly]

## Research scope note
[How many angles/sources were covered; flags if a dedicated deep-research
mode would go further on a genuinely exhaustive topic]

## Sources
[Full list, tagged by which sub-question(s) they supported]
```

## Domain coverage

The credibility rubric and source-weighting guide both carry domain-specific heuristics for: technical/developer research, market/financial research, health/medical research, policy/legal research, and general consumer research (products, travel, lifestyle). Each domain has its own source-priority order — e.g. primary filings before analyst notes for financial topics, official docs before tutorials for technical topics, the actual legislative text before advocacy framing for policy topics.

## Installation

Upload the `.skill` file to Claude (claude.ai Settings → Capabilities → Skills, or via the Claude API/Claude Code skills directory). Claude will load `SKILL.md` automatically when a query matches the trigger conditions, and pull in the relevant `references/` file only when it reaches that stage.

## Design notes

- **No hard call ceiling.** Budgets in the table are starting estimates, not contracts. If a topic is more contested or complex than expected, the pipeline is explicitly allowed to run past the original estimate — it just has to say so in the scope note rather than silently truncating.
- **Disagreement isn't a bug to hide.** Stage 5 and the "Contested or uncertain" section exist specifically so that when sources genuinely conflict, that conflict survives into the output instead of getting flattened by whichever source got read first.
- **Pairs naturally with Hallucination Guard.** Stage 8 here is a research-specific citation trace; Hallucination Guard's reasoning-integrity pass is a complementary, more general check and can be layered on top for extra-high-stakes reports.

# Query Decomposition Playbook

Used in Stage 2 of the Research Excavator pipeline. Worked examples for turning each Stage 1 classification into sub-questions, with realistic call-budget guidance.

## Factual lookup

**Example**: "What's the current corporate tax rate in Ireland?"

This needs almost no decomposition — it's one fact. Sub-questions:
1. What is the current headline rate?
2. Has it changed recently, or is there a pending change worth flagging?

Budget: 2–4 calls. Resist the urge to over-research a simple question — running 6 angles on a single-fact lookup is the over-engineering version of the search-thrashing failure mode, just dressed up as thoroughness.

## Comparative analysis

**Example**: "Postgres vs DynamoDB for a high-write IoT telemetry pipeline"

Decompose by giving each option its own angle, plus shared evaluation criteria:
1. Postgres write throughput characteristics and scaling pattern for this workload shape
2. DynamoDB write throughput characteristics and scaling pattern for this workload shape
3. Cost model comparison at the relevant scale
4. Operational overhead comparison (managed vs. self-managed, schema flexibility needs)
5. Real-world reports of either choice failing or succeeding at this kind of workload specifically (not generic marketing comparisons)

Budget: 8–15 calls. Angle 5 is the one most people skip and the one that actually surfaces contradictions worth reporting — generic "Postgres vs DynamoDB" comparison content is everywhere and mostly says the same generic things; workload-specific reports are where the real signal is.

## Open-ended exploration

**Example**: "State of the AI coding agent market"

No single right decomposition — pick angles that together cover the space without huge overlap:
1. Major players and their positioning
2. Recent funding/M&A activity (signals where money thinks the value is)
3. Technical approach differences (agentic loops vs. autocomplete vs. full-repo context, etc.)
4. Adoption signals (what are actual developers saying, not just vendor claims)
5. Open problems / what's still clearly unsolved

Budget: 12–20 calls. This classification is the most likely to legitimately run long — that's fine, just say so in the Stage 7 scope note rather than artificially truncating to hit a budget that doesn't fit the question.

## Decision support

**Example**: "Should I refinance my mortgage now?"

Decompose around what actually changes the decision, not just "facts about refinancing" in general:
1. Current rate environment and near-term trajectory signals
2. Break-even math given the user's actual numbers (rate, balance, time horizon — ask if not given)
3. Costs/fees typically involved that affect the break-even calculation
4. Situations where refinancing is commonly a mistake even when rates look favorable (e.g., resetting the clock on a mortgage near payoff, prepayment penalties)

Budget: 10–18 calls. Decision-support questions need angle 4 specifically — most content optimizes for "here's why refinancing is good" because that's what gets clicks; the failure modes are the part worth digging for.

## General rule of thumb

If you're not sure which classification fits, look at what changes if the answer is wrong. A wrong factual lookup is just an inaccuracy. A wrong comparative or decision-support answer can lead someone to actually make a bad call — that asymmetry is itself a reason to lean toward more angles, not fewer, whenever you're unsure.

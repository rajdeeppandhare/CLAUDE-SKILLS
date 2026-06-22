# History Lens

A decision analysis engine that routes your decision to 1–2 matched company archetypes, surfaces the *specific* historical analog (not the most famous one — the right one), explains why it worked structurally for them, strips it to a transferable principle under your actual constraints, forces disagreement between lenses to surface the real tradeoff, and adds the graveyard countercase to kill survivorship bias.

This is not a "what would Amazon do" platitude generator. It's built to stop you from borrowing a company's *behavior* without the structural resources that made that behavior work for them.

---

## Why this exists

"What would Amazon do?" usually produces a vibe-matched answer — "they'd take the long view," "they'd play the long game" — with no actual causal model behind it. That's not useful advice, it's a fortune cookie with a logo on it.

The core flaw it fixes: most strategic borrowing skips straight from *what a company did* to *what you should do*, without ever asking *why it worked for them* — which usually depends on resources (board control, capital access, monopoly position, distribution) you don't have. Skipping that step is how bad strategy gets made.

## How it works

Six-phase pipeline, run in order:

1. **Decision Intake** — lock in the actual decision and its constraints (time horizon, runway, team size, core question type) before picking any lens.
2. **Archetype Matching** — pick 1–2 of the 5 company archetypes that genuinely match the *type* of decision. Never spray all 5.
3. **Lens Analysis** — for each matched archetype: the historical analog, why it worked structurally for them, the transferable principle stripped of their resources, and a mandatory graveyard countercase (their relevant failure).
4. **Lens Disagreement** — explicitly surface where the matched lenses give contradictory advice. The disagreement is the insight — it's never resolved by picking a winner.
5. **Constraint Re-derivation** — take the transferable principles and re-derive what they actually imply under *your* constraints, not the original company's.
6. **Recommendation** — a single concrete recommendation, the tradeoff being made, and a Decision Audit handoff block (key assumption, archetype followed, archetype rejected) so the bet can be graded later.

## The five archetypes

| Archetype | Core muscle | Right question to apply it to |
|---|---|---|
| **Amazon** | Long-horizon bet under public pressure | Should I sacrifice near-term metrics for infrastructure/platform advantage? |
| **Apple** | Ruthless prioritization / saying no | What should I cut? What's actually core? |
| **Microsoft** | Platform/ecosystem leverage | How do I embed in other people's stacks? How do I become infrastructure? |
| **Nvidia** | Betting on a thesis before the market agrees | Should I invest in something the market doesn't yet value? |
| **Stripe** | Default infrastructure via developer obsession | Am I competing on features, or on integration quality and trust? |

Each archetype's reference file (`references/*.md`) contains 3 win-case studies and 1 graveyard failure case, each with the specific decision, year, structural enabler, and transferable principle already worked out — so the skill pulls from real analysis rather than improvising history on the fly.

## Example trigger phrases

- "What would Amazon do here?"
- "How would Apple approach this decision?"
- "What would history do?"
- "Help me think through this using historical company analogs"
- "Pressure-test this decision against how great companies have handled similar calls"

## What it will refuse to do

- Run all 5 lenses on every decision (spray pattern, not analysis — it narrows to 1–2)
- Give you a transferable principle without first explaining the structural enabler behind it
- Skip the graveyard countercase
- Resolve a lens disagreement by declaring one side "correct"
- Hand you a recommendation that isn't re-derived from your actual constraints (if the advice would be identical for Amazon and a solo founder, it hasn't done its job)

## File structure

```
history-lens/
├── SKILL.md                  # Main skill definition — pipeline, guardrails, anti-patterns
├── README.md                 # This file
├── history-lens.skill        # YAML manifest
└── references/
    ├── amazon.md              # Win cases: AWS, Prime, 3rd-party marketplace. Graveyard: Fire Phone
    ├── apple.md               # Win cases: 1997 SKU purge, dropping the floppy, killing 30-pin. Graveyard: Newton
    ├── microsoft.md           # Win cases: DOS licensing, backward compatibility, Nadella cloud pivot. Graveyard: Windows Phone
    ├── nvidia.md               # Win cases: CUDA, tensor-core data center bet, resisting the crypto boom. Graveyard: Tegra mobile
    └── stripe.md               # Win cases: 7-lines-of-code integration, docs-as-product, Atlas. Graveyard: Bitcoin payments
```

## Usage notes

- Decision Intake (Phase 1) needs a *concrete, committed action*, not a vague direction. "Should I cut our enterprise tier?" works. "How should we think about growth?" doesn't — the skill will push back and ask you to sharpen it.
- If you name a company outside the 5 archetypes (e.g. "what would Netflix do"), the skill will map it to the closest matching archetype's *muscle* (not the company name) or ask you to clarify which underlying behavior you actually want analyzed.
- The Decision Audit handoff block at the end is designed to log into a separate Decision Quality / Decision Audit tracking skill, if you're running one — it's the part that turns this from a one-off framing exercise into something you can grade later against what actually happened.

## License

MIT

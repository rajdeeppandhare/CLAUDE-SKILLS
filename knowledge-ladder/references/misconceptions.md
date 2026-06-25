# Misconception Library

Common wrong beliefs by domain. Surface these when the user's phrasing signals a misconception. Always correct non-condescendingly and explain *why* the misconception exists — this builds trust.

Format when surfacing:
```
⚠️ Common misconception

What people often think: [their belief]
What's actually true: [correction]
Why this misconception exists: [brief — 1 sentence]
```

---

## AI / Machine Learning

| Misconception | Reality | Why it exists |
|--------------|---------|---------------|
| "AI is intelligent / thinks like a human" | AI predicts patterns in data. It has no understanding, goals, or consciousness. | The outputs *look* like thinking, so we infer the process must be similar. |
| "More data always makes models better" | More data helps up to a point. Noisy, irrelevant, or biased data actively hurts performance. | "More is better" is a useful heuristic in most domains, but not here. |
| "AI will give the right answer if you ask nicely" | LLMs generate plausible-sounding text. Politeness doesn't affect accuracy. | Human social dynamics don't apply to statistical models. |
| "Training and inference are the same thing" | Training = learning from data (expensive, once). Inference = applying the learned model (cheap, repeated). | The word "running AI" blurs both. |
| "Neural networks work like the human brain" | Inspired by neurons conceptually. Mechanistically, they're matrix multiplications — nothing like biology. | The name "neural" implies biological similarity. |
| "A higher accuracy always means a better model" | On imbalanced datasets, 99% accuracy can mean the model ignores the rare class entirely. | Accuracy sounds like the obvious measure; it isn't always. |
| "AI can reason from first principles" | Current LLMs pattern-match on training data. They can mimic reasoning steps but can't derive from axioms. | Multi-step outputs look like reasoning chains. |
| "Bigger model = smarter model" | Scaling helps, but returns diminish. A well-trained small model can outperform a poorly-trained large one. | Size is a visible, measurable proxy for quality. |

---

## Finance / Investing

| Misconception | Reality | Why it exists |
|--------------|---------|---------------|
| "Stock price = company value" | Price is what someone paid last. Value is what the business is worth. They diverge constantly. | Price is visible and immediate; value requires analysis. |
| "Diversification eliminates risk" | It eliminates idiosyncratic (single-stock) risk. Systematic (market-wide) risk remains. | "Don't put all eggs in one basket" is taught as universal truth. |
| "Past returns predict future returns" | For individual stocks and most funds: no. For asset classes over long horizons: weakly. | Fund marketing prominently features past returns. |
| "A high P/E means a stock is expensive" | P/E must be compared to growth rate (PEG), sector peers, and interest rates. High P/E can be cheap for fast growers. | P/E is the simplest ratio to calculate. |
| "Inflation is always bad" | Moderate inflation (2-3%) is a sign of healthy demand. Deflation is often more damaging. | Rising prices feel intuitively bad. |
| "Day trading is investing" | Day trading is speculation. Investing is allocating capital to productive assets over time. | Both involve buying and selling — the difference is time horizon and intent. |

---

## Physics

| Misconception | Reality | Why it exists |
|--------------|---------|---------------|
| "Gravity pulls things down" | Gravity curves spacetime. Objects follow the straightest possible path through curved space. "Down" is a local approximation. | "Down" works perfectly at human scales; the deeper picture is hidden. |
| "Objects in space are weightless because there's no gravity" | Gravity still acts in orbit. Astronauts are weightless because they're in free fall — constantly falling around Earth. | "Space" implies escape from Earth's pull. |
| "Heat rises" | Hot air rises (lower density). Heat itself flows from hot to cold in all directions. | We observe the air, not the heat. |
| "Electrons orbit the nucleus like planets orbit the sun" | Electrons exist in probability clouds (orbitals), not fixed paths. | The Bohr model is taught first; it's intuitive but wrong. |
| "Entropy means disorder" | Entropy is the number of microstates consistent with a macrostate. "Disorder" is a useful but imprecise metaphor. | Disorder is a relatable proxy for an abstract statistical concept. |

---

## Computer Science

| Misconception | Reality | Why it exists |
|--------------|---------|---------------|
| "More RAM = faster computer" | RAM affects what fits in memory. CPU speed, cache, and algorithm efficiency often matter more. | RAM is a visible, upgradeable spec. |
| "Blockchain is the same as Bitcoin" | Blockchain is a data structure. Bitcoin is one application of it. | Bitcoin made blockchain famous. |
| "Encryption = security" | Encryption is one layer. Security also requires: key management, authentication, authorization, software bugs, human factors. | Encryption sounds like a complete solution. |
| "Binary search is always the fastest search" | Only on sorted data. For unsorted, hash lookup is O(1) vs binary search's O(log n). | Binary search is taught as the canonical "smart" search. |
| "The cloud is someone else's computer" | True at a basic level. In practice, it's programmable infrastructure with massive economies of scale and specialized hardware. | The joke is technically accurate but misses the engineering sophistication. |

---

## Biology / Health

| Misconception | Reality | Why it exists |
|--------------|---------|---------------|
| "We only use 10% of our brain" | All regions are active, just not all simultaneously. Brain tissue is metabolically expensive — unused regions would atrophy. | Misattributed quotes and pop science articles. |
| "DNA is your destiny" | Gene expression is regulated by environment (epigenetics). Most traits are polygenic and environmental. | "It's in your genes" is a clean, deterministic story. |
| "Viruses are alive" | Viruses lack metabolism and can't self-replicate outside a host — most definitions of life require both. | They behave strategically, which feels like life. |
| "Antibiotics treat viruses" | Antibiotics target bacterial cell structures. Viruses have no cell wall or ribosomes — antibiotics can't touch them. | Both cause "being sick," so the fix feels interchangeable. |

---

## Economics

| Misconception | Reality | Why it exists |
|--------------|---------|---------------|
| "Trade is zero-sum" | Voluntary exchange is positive-sum — both parties gain or they wouldn't trade. | One party winning feels like the other losing. |
| "Minimum wage increases always cause unemployment" | Evidence is mixed. Small increases in low-elasticity labor markets often have minimal employment effects. | Simple supply/demand model predicts it cleanly. |
| "Government debt is like household debt" | Governments can print currency, refinance indefinitely, and owe mostly to their own citizens. Households cannot. | The analogy feels intuitive and is widely used in political rhetoric. |
| "GDP measures wellbeing" | GDP measures economic output. It ignores inequality, health, leisure, environmental degradation, and unpaid work. | It's the most-cited economic statistic. |

# Gap Scoring Rubric

## The Formula

```
Gap Score = (Pain Severity × Segment Size) / Why-Nobody-Does-This Risk
```

Score range: 0–10. Gaps below 4.0 are dropped or flagged. Gaps 7.0+ are primary briefs.

---

## Dimension 1: Pain Severity (1–5)

Measures how real and urgent the pain is. High severity = unprompted, repeated, emotional.

| Score | Description | Evidence signal |
|---|---|---|
| 1 | Theoretical pain | Nobody mentions it in reviews; you inferred it from a feature gap |
| 2 | Mild inconvenience | Occasional "would be nice" in reviews; no strong sentiment |
| 3 | Moderate friction | Mentioned regularly in reviews; some workarounds described |
| 4 | Real pain | Appears in 5+ reviews across sources; users describe workarounds or switching |
| 5 | Acute pain | Appears in 10+ reviews; drives churn or blocks adoption; explicit in forum threads |

**Scoring rules:**
- Only score 4+ if evidence is from user-generated content (reviews, forums), not your inference
- Score 5 only if the pain crosses multiple platforms (G2 + Reddit + Capterra, not just one)
- "Would be nice" language → max score of 2
- "We switched because of this" or "deal-breaker" language → minimum score of 4

---

## Dimension 2: Segment Size (1–5)

Measures how many buyers share this pain. High = large addressable segment.

| Score | Description | Evidence signal |
|---|---|---|
| 1 | Niche of a niche | Fewer than 1,000 potential buyers globally |
| 2 | Small segment | Thousands of buyers; may be viable for bootstrapping, not VC |
| 3 | Meaningful segment | Tens of thousands of buyers; real business possible |
| 4 | Large segment | Hundreds of thousands of buyers; multiple companies could win here |
| 5 | Mass market | Millions of potential buyers; category-defining opportunity |

**Scoring rules:**
- Don't score 5 without a rough size estimate (TAM logic or proxy)
- SMB in a common software category (CRM, PM, HR) = 4 minimum
- Niche vertical with no adjacent markets = maximum 2
- Geographic segment in a non-English market with >50M population = 3 minimum

**Proxy signals for size:**
- Search volume for the pain/use-case (Google Trends, Ahrefs)
- Reddit community size for the persona
- Number of reviews on G2/Capterra for the category (proxy for total buyers)
- Job posting volume for the buyer persona

---

## Dimension 3: Why-Nobody-Does-This Risk (1–5)

This is the denominator — it divides the score. Higher risk = lower score. Low risk = gap is genuinely open.

| Score | Description |
|---|---|
| 1 | No obvious reason this is hard; seems like an oversight or GTM choice |
| 2 | Minor structural reason (sales motion, pricing model) that a new entrant could solve |
| 3 | Moderate structural reason (architecture, compliance, CAC economics) — possible but costly |
| 4 | Major structural reason — incumbents likely tried and struggled; high rebuild risk |
| 5 | Graveyard confirmed — multiple companies tried, none succeeded; structural failure mode |

**Scoring rules:**
- Default to 2 unless you have evidence of a structural reason
- Score 4+ only with evidence: ex-competitors who exited, known CAC problems, confirmed architectural constraints
- Score 5 only if graveyard check confirms prior failures

---

## The Graveyard Check (Mandatory for gaps scoring ≥7.0 before final scoring)

Before elevating a gap to a primary brief, run the graveyard check:

**Step 1: Search for prior attempts**
- `"[segment]" "[category]" startup`
- `"[use case]" site:news.ycombinator.com`
- `"[competitor]" "discontinued" OR "shut down" OR "pivoted"`
- Check Crunchbase for companies that raised funding in this space and went quiet

**Step 2: Look for structural failure signals**
- CAC>LTV: Did SMB-focused versions of enterprise tools fail because acquisition was expensive but revenue was low?
- Churn: Did low-complexity products targeting non-technical users fail due to low engagement?
- Regulation: Was a market abandoned due to compliance overhead?
- Platform risk: Was a segment abandoned because it depended on a platform that changed terms?

**Step 3: Classify the graveyard**
| Type | Implication |
|---|---|
| Failed for CAC reasons | Validate your go-to-market before acting; the gap may require a different distribution channel |
| Failed for churn reasons | Validate retention thesis; your product must solve the churn cause, not just match features |
| Failed for platform/regulation reasons | Check if those conditions changed; if yes, gap reopened |
| Failed for product reasons (wrong solution) | Gap may still be open if you have a better solution |
| No evidence of prior attempts | Lowest risk; likely an oversight by incumbents |

---

## Scoring Examples

**Example A:**
- Gap: SMB ops teams ignored by all enterprise-priced competitors
- Pain Severity: 4 (14 reviews mention complexity for small teams)
- Segment Size: 4 (SMB ops is a large segment)
- Risk: 2 (no graveyard evidence; pricing/sales-motion issue, not structural)
- **Score: (4×4)/2 = 8.0 → Primary brief**

**Example B:**
- Gap: Left-handed UX accommodations
- Pain Severity: 1 (no review evidence; theoretical)
- Segment Size: 1 (niche)
- Risk: 1
- **Score: (1×1)/1 = 1.0 → Dropped**

**Example C:**
- Gap: HIPAA-compliant version for healthcare SMBs
- Pain Severity: 3 (mentioned in forums, not dominant)
- Segment Size: 3 (real segment, but compliance limits scale)
- Risk: 4 (HIPAA compliance is expensive; BAA requirements are a known friction point)
- **Score: (3×3)/4 = 2.25 → Flagged, not a primary brief; note graveyard risk**

---

## Score Interpretation

| Score | Action |
|---|---|
| 8.0–10.0 | Primary brief — top recommendation |
| 6.0–7.9 | Secondary brief — worth pursuing with more validation |
| 4.0–5.9 | Monitor — real signal but insufficient evidence or high risk |
| Below 4.0 | Drop — do not include in output |

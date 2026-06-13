# Decision OS — Worked Examples

Full end-to-end examples showing how the pipeline runs for real decisions.

---

## Example 1: MBA vs Startup

**User input:**
"Should I pursue an MBA at a top school or quit my job and build a startup?
I'm 28, have been a PM at a Series B startup for 3 years, have about $40k saved,
and a SaaS idea I'm pretty excited about. The MBA would be at Wharton / Booth tier."

---

### Stage 0 — Intake

```
Decision:        MBA (Wharton/Booth tier) vs found a startup
Options:         Option A: MBA | Option B: Startup
Decision Type:   Career + Entrepreneurship
Time Horizon:    Long (3yr+)
Reversibility:   Costly to reverse (MBA = 2yr + $200k; Startup = window may close)
Stakes:          Life-defining
User's gut lean: Startup (mentioned excitement; MBA framed as the "safe" comparison)
```

---

### Stage 1 — Weighted Decision Matrix

```
Criteria                Weight   MBA    Startup   WScore MBA   WScore S
──────────────────────────────────────────────────────────────────────
Growth trajectory        0.20    8/10    9/10       1.60        1.80
Financial upside         0.15    6/10    9/10       0.90        1.35
Skill development        0.20    7/10    9/10       1.40        1.80
Optionality unlocked     0.15    9/10    6/10       1.35        0.90
Network quality          0.10    9/10    5/10       0.90        0.50
Identity alignment       0.10    5/10    8/10       0.50        0.80
Risk profile             0.10    8/10    5/10       0.80        0.50
──────────────────────────────────────────────────────────────────────
TOTAL                    1.00                       7.45        7.65
```

**Matrix verdict: Startup (narrow, 0.20 margin) → frameworks needed**

---

### Stage 2 — Opportunity Cost

Choosing MBA costs you:
- 24 months of product-building compounding experience
- The specific window for your SaaS idea (markets time-sensitive)
- ~$200k direct cost + $180k foregone salary = $380k+ true cost
- The founder identity and learning you'd only get by actually doing it

Choosing Startup costs you:
- The Wharton/Booth network (worth $X over a career — hard to replicate)
- The credential that opens PE/VC/consulting doors
- 2 structured years to learn frameworks, finance, strategy
- The safety net of a degree if startup fails

**Key insight:** The MBA keeps more conventional doors open. The startup tests
whether the door you actually want — being a founder — is real.

---

### Stage 3 — Risk Scoring

```
                    MBA                     Startup
──────────────────────────────────────────────────────────────────────
Upside:             Strong career, network   Life-changing outcome
                    PE/VC/C-suite access     Financial independence
Probability:        75% of stated upside     15–20% of big outcome

Downside:           Debt, opportunity cost   Run out of money,
                    Wrong career direction   idea fails, 18mo lost
Probability:        20%                      60–70%

Catastrophe:        Minimal — degree is      Reputational damage
                    always recoverable       minimal at 28; financial
                                             damage limited by savings
Probability:        <5%                      5%
──────────────────────────────────────────────────────────────────────
Risk Profile:       Symmetric, moderate      Positively asymmetric
                    upside/downside          (big upside, survivable down)
```

**Key finding:** The startup has **positive asymmetry** — the downside is
survivable and recoverable at 28 with no dependents. The MBA has **symmetric**
risk — reliable upside, but also meaningful downside (wrong direction, debt).

---

### Stage 4 — Time & Energy

MBA requires:
- Year 1: Full immersion, recruiting season (fall), case competitions
- Year 2: Internship, thesis, second recruiting push
- Peak load: October–February both years
- Life impact: Social life constrained by budget and schedule; career direction locked in

Startup requires:
- Months 1–6: Product + validation (intense, variable hours)
- Months 6–18: If traction, fundraise (different but equally intense)
- Months 18+: Either scaling (exciting) or pivoting/shutting down (clarifying)
- Hidden time debt: Fundraising rounds consume founder focus for 3–6 months each

**Energy ROI observation:** Users who describe startup ideas with excitement ("pretty excited")
tend to report far higher energy ROI from building than from study — even in the
hard periods. This is a weak signal but worth noting.

---

### Stage 5 — Regret Minimisation

```
Scenario A: MBA — works
  You're 32, at a great PE fund or leading product at a major company.
  Credential opened doors. Network is gold. Strong platform to do almost anything.
  Regret score: 2/10 (low — would feel like a good call)

Scenario A: MBA — disappoints
  The job you get isn't the one you imagined. Debt is heavy. You wonder
  if you should have tried the startup when you had the energy and savings.
  Regret score: 6/10

Scenario B: Startup — works
  You're 30, running a real product with real revenue. You learned more in
  18 months than 3 years of PM work. The MBA option is still open if you want it.
  Regret score: 1/10

Scenario B: Startup — fails
  You're 30, out of savings, back to a job. But you know. You tried.
  The idea was tested. No lingering "what if."
  Regret score: 3/10

Scenario C: Neither — kept waiting
  You're 30 still PM-ing, still thinking about both, still haven't moved.
  Regret score: 8/10 ← highest
```

**Regret verdict: Startup** — even the failure scenario is lower regret than
the MBA disappointment scenario.

---

### Stage 6 — Identity Alignment

This decision touches:
- Professional identity: "Builder" vs "Credentialed professional"
- Financial identity: Risk-taker (limited savings, betting on self) vs security-seeker
- Freedom: Startup = high autonomy | MBA = structured path

MBA aligns with: Long-term optionality, credential status, network-building drive
MBA conflicts with: Builder identity, need for immediate agency, excitement about the specific idea

Startup aligns with: Builder identity, excitement, direct ownership of outcomes
Startup conflicts with: Financial security needs (low savings is a constraint)

**Identity finding:** The language used ("pretty excited about the idea") signals
builder identity. The MBA is framed defensively ("the safe option"). When one path
is described with energy and the other with safety framing, identity alignment
usually points to the energising option.

---

### Stage 7 — Second-Order Consequences

MBA path:
1st order → Credential, network, career capital
→ 2nd order → High-paying role + debt obligation shifts risk tolerance conservatively
→ 3rd order → Optimise for income to service debt; entrepreneurial window feels smaller

Startup path:
1st order → Product built, idea tested
→ 2nd order → If traction: Series A, team, real company. If not: clarity + credibility + MBA still possible
→ 3rd order → Either a real company, or a battle-tested founder who's more valuable to any org

**Non-obvious consequence:** A failed startup at 28 with a real product, real customers
attempted, and a clear shut-down often makes you *more* hireable and MBA-admissible,
not less. The fear of the downside is empirically overstated.

---

### Stage 8 — Pre-Mortem

Pre-Mortem: Startup fails

Most likely failure modes:
1. Ran out of money before finding product-market fit (~50% probability)
2. Built something technically but couldn't sell it (~25%)
3. Lost conviction and returned to employment (~15%)
4. Co-founder or team conflict (~10% if solo-founding, higher otherwise)

Early warnings (3 months):
- No paying users / no one willing to pilot for free
- You're building features instead of talking to customers
- You've been avoiding the sales/distribution problem

Mitigation:
- Set a hard go/no-go checkpoint at month 6 (specific metrics)
- Pre-commit to consulting/contract work as a bridge before full savings depletion
- Validate demand before quitting — get 3 people to say "I will pay for this"

---

### Stage 9 — Synthesis

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DECISION OS — SYNTHESIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Decision:       MBA (Wharton/Booth) vs Startup (SaaS idea)
Frameworks run: All 9

Framework scoreboard:
  Matrix:          → Startup (narrow)
  Opportunity cost → Startup
  Risk profile:    → Startup (positive asymmetry)
  Time ROI:        → Startup
  Regret:          → Startup (clear)
  Identity fit:    → Startup
  2nd order:       → Startup
  Pre-mortem:      → Manageable for both; Startup failure is recoverable

VERDICT:        Startup — with a structured 6-month validation gate
Confidence:     Medium-High

The insight you might have missed:
  The MBA is an insurance policy for a life you're not sure you want.
  The startup is a bet on the life you actually described wanting. At 28,
  with no dependents and $40k saved, you can afford to test the bet — and
  a failed startup doesn't close the MBA door. A 2-year MBA + debt does
  meaningfully narrow the startup window.

Key condition:
  If you have a co-founder or early customer validation already, confidence
  moves to High. If you're completely solo and pre-validation, do 90 days
  of evenings/weekends validation before quitting — reduce the largest
  risk factor before going full-time.

If you choose Startup:
  Do this first: Get 3 prospective users to commit (even verbally) to paying
                 before you hand in your notice.
  Watch for:     At month 6 — if you have zero paying users, revisit the decision.

If you choose MBA:
  Make sure: You can name 5 specific opportunities that only exist via this
             path that you genuinely want and can't get otherwise.
  Biggest risk: Taking on $200k+ debt to delay a question you haven't answered.

Cognitive biases detected:
  Status quo bias — the MBA is framed as "the safe option" but $200k debt +
  2 years is itself a major commitment, not a neutral baseline.

  Loss aversion — the savings constraint ($40k) is mentioned as a risk, but
  $40k in runway at lean burn rates is 12–18 months. That's more time than
  most people think they have.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Example 2 — Quick Low-Stakes Example (Reversible Decision)

**User input:** "Should I switch from Notion to Obsidian for my notes?"

**Stage 0:** Reversible, Low Stakes → run condensed version

**Response:**
This is a cheap experiment — you can try Obsidian for 30 days and switch back
with minimal cost. Rather than a full analysis:

Matrix (quick):
- Local-first / longevity: Obsidian wins
- Collaboration: Notion wins
- Setup effort: Notion wins
- Power / flexibility: Obsidian wins

Regret: Neither choice has meaningful regret attached to it.

**Verdict:** If you work mostly alone and want data ownership long-term, try Obsidian
for 30 days. The real answer is 30 days of data, not a decision matrix.

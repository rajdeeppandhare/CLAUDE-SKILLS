# Reddit Miner — Opportunity Engine

Deep opportunity scoring, startup validation signals, and market sizing intelligence.
Load this when the user wants to go beyond complaint listing into actionable opportunity analysis.

---

## Table of Contents

1. [Opportunity Scoring Framework](#1-opportunity-scoring-framework)
2. [Startup Validation Signals](#2-startup-validation-signals)
3. [Market Sizing from Reddit](#3-market-sizing-from-reddit)
4. [Competitor Weakness Mapping](#4-competitor-weakness-mapping)
5. [Monetisation Pattern Library](#5-monetisation-pattern-library)
6. [Idea Stress-Testing](#6-idea-stress-testing)

---

## 1. Opportunity Scoring Framework

Every opportunity identified from Reddit signals gets scored across 5 dimensions.
Score each 1–10. Weight and sum for a final Opportunity Score.

### Dimensions

**Frequency (30% weight)**
How often is this pain mentioned, independently, across communities?

| Score | Signal |
|---|---|
| 1–3 | Mentioned once or twice, possibly one vocal user |
| 4–6 | Appears in multiple threads, some from different users |
| 7–9 | Recurring theme across subreddits and timeframes |
| 10 | Dominant complaint — hard to miss, mentioned constantly |

**Intensity (25% weight)**
How emotionally charged are the mentions?

| Score | Signal |
|---|---|
| 1–3 | Mild preference — "it'd be nice if..." |
| 4–6 | Clear frustration — "this is annoying because..." |
| 7–9 | Genuine pain — "I've lost X / wasted hours / this killed my project" |
| 10 | Fury — multiple rants, people actively seeking alternatives |

**Market Readiness (20% weight)**
Are there signals that people are ready to pay to solve this?

| Score | Signal |
|---|---|
| 1–3 | No payment signals, unclear if they'd pay |
| 4–6 | Some mentions of trying paid alternatives, or DIY solutions |
| 7–9 | Active discussion of willingness to pay, mentions of specific price tolerance |
| 10 | Explicit "I would pay for X", mentions of failed existing paid solutions |

**Competition Gap (15% weight)**
How underserved is this pain by existing solutions?

| Score | Signal |
|---|---|
| 1–3 | Multiple well-regarded tools exist, community is happy with options |
| 4–6 | Tools exist but have significant known flaws |
| 7–9 | Existing tools are clearly inadequate or hated |
| 10 | No real solution exists — people are cobbling together workarounds |

**Trend Direction (10% weight)**
Is this pain growing, stable, or fading?

| Score | Signal |
|---|---|
| 1–3 | Fading — fewer mentions in recent vs older posts |
| 4–6 | Stable — consistent level of discussion |
| 7–9 | Growing — noticeably more mentions in recent 6–12 months |
| 10 | Exploding — dramatically more mentions, emerging market signal |

### Score Calculation

```
Opportunity Score = 
  (Frequency × 0.30) +
  (Intensity × 0.25) +
  (Market Readiness × 0.20) +
  (Competition Gap × 0.15) +
  (Trend Direction × 0.10)
```

### Interpretation

| Score | Verdict |
|---|---|
| 8.5–10 | 🔥 Build this now. Strong market signal. |
| 7–8.4 | ✅ Strong opportunity. Validate with a landing page first. |
| 5–6.9 | ⚠️ Moderate signal. Niche may be too small or too early. |
| 3–4.9 | 🔍 Weak signal. Further research needed before investing. |
| 1–2.9 | ❌ Skip. Low signal or saturated market. |

---

## 2. Startup Validation Signals

### Green Flags (build confidence)

These Reddit patterns indicate strong validation:

- **Workaround posts** — People describing DIY solutions = market exists, no good product
- **"Does X exist?" posts** — Explicit demand for something that doesn't exist yet
- **Competitor complaint patterns** — Multiple tools, all with same complaint = systemic gap
- **Freelancer / agency mentions** — People charging money to do this manually = should be automated
- **"I built X for myself"** — DIY solutions getting upvotes = wide demand beyond builder
- **Price complaints on expensive tools** — Clear willingness to pay, opening for affordable alternative
- **Enterprise-only tools** — Complaints about pricing = SMB / prosumer market underserved

### Red Flags (pause and investigate)

- **Only one or two vocal users** — May not represent broad market
- **Problem mentioned only by very advanced users** — May be too niche
- **The complaint is really about user error** — Education gap, not product gap
- **Existing free solution exists but isn't mentioned** — May not be a real opportunity
- **Regulatory / compliance-heavy space** — Slower to monetise, higher barrier
- **"We use enterprise tool X" is the accepted answer** — May be B2B only with long sales cycles

### The "Show HN / Validate" Post Test

When you find a strong opportunity, look for whether anyone has already validated it:
- Search for posts like "I built X, looking for feedback"
- Search for posts mentioning a startup in the space
- If someone built it and it flopped: understand WHY before pursuing
- If someone built it and it's growing: confirms demand (you'd be competing)

---

## 3. Market Sizing from Reddit

Reddit doesn't give you TAM directly, but you can triangulate:

### Subreddit Size as Proxy

| Subreddit Size | Signal |
|---|---|
| <10k members | Niche market — high specialisation, lower ceiling |
| 10k–100k | Mid-size market — viable for focused SaaS |
| 100k–1M | Large market — strong demand, likely competitive |
| 1M+ | Mass market — high competition, need strong differentiation |

### Engagement Rate as Intensity Proxy

- Average post upvotes / subreddit size = engagement rate
- High engagement on pain-point posts = strong willingness to care about solutions

### Workaround Complexity as WTP Proxy

- Simple workaround (1 step): lower WTP
- Complex workaround (5+ steps, takes hours): high WTP — people will pay to avoid this

### Cross-Subreddit Presence

Same pain appearing across **3+ different subreddits** = the pain transcends a single niche.
That's a strong horizontal market signal.

---

## 4. Competitor Weakness Mapping

When competitors are mentioned in Reddit threads, extract structured intelligence:

### Weakness Taxonomy

**Category 1: Core Product Gaps**
- Missing features that users explicitly ask for
- Functionality that exists but is broken or unreliable
- Performance issues (speed, reliability, uptime)

**Category 2: UX / Usability**
- Steep learning curve
- Poor mobile experience
- Confusing navigation / information architecture
- Onboarding that causes early churn

**Category 3: Pricing & Business Model**
- Price point too high for target market
- Per-seat pricing that punishes growth
- Feature gating that feels artificial / predatory
- Surprise charges, poor billing UX

**Category 4: Support & Community**
- Slow / unhelpful support
- Documentation that's incomplete or outdated
- Community that's hostile to beginners
- Slow to respond to bug reports

**Category 5: Trust & Reliability**
- Data loss incidents
- Privacy / security concerns raised
- Company direction uncertainty (acquisition rumors, pivots)
- Sudden pricing changes

### The "Switcher Profile"

When people describe switching FROM a competitor, extract:
- What was the trigger event? (usually one specific thing, not a gradual drift)
- What did they try before switching? (shows switching cost tolerance)
- What would make them go back? (tells you the competitor's true core value)

---

## 5. Monetisation Pattern Library

Match these patterns to the opportunities found:

### B2C SaaS

**Freemium → Pro**
Works when: Core value can be experienced free, power users hit a natural limit
Reddit signal: "I wish the free plan included X"

**One-time purchase**
Works when: Tool is used occasionally, users resistant to subscriptions
Reddit signal: "I'd pay $X once but not monthly"

**Usage-based**
Works when: Value scales with usage, users prefer to start small
Reddit signal: Power users clearly extracting more value than casual users

### B2B SaaS

**Per-seat**
Reddit signal: Team use mentioned, "we use this at work"

**Per-output / API**
Reddit signal: Developers discussing volume, automation use cases

**Enterprise contract**
Reddit signal: "We've been trying to get IT to approve" — long buying cycles

### Content / Community

**Newsletter / Paid community**
Reddit signal: People asking "where can I learn more about X from real practitioners"

**Course / Bootcamp**
Reddit signal: Recurring question "where do I even start with X" from beginners

**Coaching / Services**
Reddit signal: "I'd pay someone to just do this for me"

---

## 6. Idea Stress-Testing

Before committing to an opportunity, stress-test it against these questions drawn from Reddit patterns:

### The 5 Killers

**Killer 1: The "Good Enough" Problem**
Is there a free, imperfect solution people already use and are fine with?
Signal: If the workaround gets upvotes and "this works for me" — need to offer 10x, not 2x.

**Killer 2: The Audience Size Problem**
Is this pain felt by enough people at a price point that works?
Signal: If the subreddit is <5k members and the pain is niche, the market may be too small for SaaS.

**Killer 3: The "Build Once, Use Once" Problem**
Is this a recurring pain or a one-time problem?
Signal: Subscription requires recurring pain. One-time tools need one-time pricing.

**Killer 4: The Distribution Problem**
Can you reach these people without depending on the platforms that host this content?
Signal: Check if there are email lists, newsletters, Discord servers, or offline communities
where these people congregate beyond Reddit.

**Killer 5: The "Power User Trap"**
Is the pain only felt by advanced users who would rather build it themselves?
Signal: If the people complaining are also developers, they'll build it themselves or expect it free.

### The Stress-Test Output Format

```
IDEA STRESS TEST: {Opportunity Name}

✅ PASSES:
  → {Killer N}: {Why this isn't a problem here}

⚠️ CONCERNS:
  → {Killer N}: {Risk and how you might mitigate it}

❌ FAILS:
  → {Killer N}: {Why this might kill the idea}

VERDICT: {Proceed / Validate first / Avoid}
NEXT STEP: {Specific action to validate or de-risk}
```

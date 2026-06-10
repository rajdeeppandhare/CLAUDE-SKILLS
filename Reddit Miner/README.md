# 🔍 Reddit Miner — Community Intelligence Skill for Claude

> Turn Reddit into structured business intelligence. Mine pain points, discover market opportunities, validate startup ideas, and extract real user sentiment — all grounded in what actual humans say.

---

## What This Skill Does

Reddit Miner is a custom Claude skill that transforms Reddit community data into structured, actionable intelligence. Instead of generic summaries, it runs a full mining pipeline: identifying the right subreddits, fetching real posts and comments via Reddit's public API, clustering signals by type and intensity, and delivering a formatted intelligence report.

**It is significantly richer than a generic Claude web search.** Every output is evidence-grounded, cited to real threads, and transformed from raw complaints into opportunities.

---

## 8 Intelligence Modes

| Mode | Best For |
|---|---|
| 🚀 **Startup / Market** | Discovering opportunities in any topic or industry |
| 📦 **Product Manager** | Feature requests, user sentiment, roadmap signals for a product |
| 🏗️ **SaaS Founder** | Competitor weaknesses, market gaps, pricing intelligence |
| 🔬 **UX Research** | Friction points, churn triggers, mental model mismatches |
| 📣 **Content Creator** | YouTube/newsletter ideas, viral angles, audience language |
| 💼 **Career Intelligence** | Role reality, compensation, interview experiences |
| 📚 **Learning Path** | Community-validated resources, sticking points, fastest paths |
| 📈 **Trading / Finance** | Loss patterns, psychology traps, community sentiment |

---

## Example Triggers

Claude will activate this skill when you say things like:

- *"Mine Reddit for pain points in the project management space"*
- *"What do people complain about with Notion?"*
- *"Find startup ideas in the fitness tracking market"*
- *"Reddit sentiment for solopreneur tools"*
- *"What are people struggling with when learning Rust?"*
- *"Find UX issues with Linear"*
- *"What do traders on Reddit regret most?"*

---

## Repository Structure

```
reddit-miner/
├── SKILL.md                        ← Main skill file (load this into Claude)
└── references/
    ├── modes.md                    ← Full output format for all 8 modes
    └── opportunity-engine.md       ← Deep opportunity scoring & startup validation
```

---

## How to Use

### Option 1 — Claude Projects (Recommended)

1. Create a new **Project** in Claude.ai
2. Upload `SKILL.md` as a project instruction or paste its contents into the system prompt
3. Upload `references/modes.md` and `references/opportunity-engine.md` as project files
4. Start a conversation and give Claude any topic, product, or market

### Option 2 — System Prompt

Paste the contents of `SKILL.md` into your system prompt when using the Claude API or any interface that supports custom instructions.

### Option 3 — Claude.ai Custom Instructions

Add the skill description and core pipeline from `SKILL.md` to your Claude.ai custom instructions for persistent activation.

---

## The Mining Pipeline

```
PHASE 1 — RECON        Identify the right subreddits
PHASE 2 — MINE         Fetch real posts + comments via Reddit's public JSON API
PHASE 3 — SYNTHESISE   Cluster signals: PAIN, FRICTION, WISH, WORKAROUND, ABANDON...
PHASE 4 — TRANSFORM    Complaints → Opportunities with scoring
PHASE 5 — REPORT       Structured intelligence in your chosen mode
```

---

## Sample Output (Startup / Market Mode)

```
╔══════════════════════════════════════════════════════════╗
║              REDDIT MINER — INTELLIGENCE REPORT          ║
╚══════════════════════════════════════════════════════════╝

🎯 TOPIC:      Solopreneur Tooling
📊 MODE:       Startup / Market
🔍 SOURCES:    r/entrepreneur, r/SideProject, r/smallbusiness (+3 more)
📥 DATA MINED: ~180 posts, ~640 comments
⏱️ SIGNAL SPAN: 2019 → present

🔥 TOP COMPLAINTS

  #1 — Tool fragmentation / "app sprawl"
      Frequency:  ████████░░  High (47+ posts)
      Intensity:  🔴 Frustrated
      Evidence:   "I'm paying for 11 different tools and they don't talk
                  to each other. I spend more time managing tools than
                  building my business."
                  — u/throwaway_founder22, r/entrepreneur, 847 upvotes

💡 MARKET OPPORTUNITY

  Name:        "SoloStack" — unified ops dashboard for solopreneurs
  Solves:      Tool fragmentation
  Signal:      ██████████  9.1/10
  Competition: Low (enterprise tools exist, nothing for 1-person businesses)
  Monetise:    $29/mo flat — no per-seat games
```

---

## Opportunity Scoring

The skill includes a full **Opportunity Engine** (`references/opportunity-engine.md`) that scores every discovered opportunity across 5 weighted dimensions:

- **Frequency** (30%) — How often is this pain mentioned?
- **Intensity** (25%) — How emotionally charged are the mentions?
- **Market Readiness** (20%) — Are people willing to pay to solve this?
- **Competition Gap** (15%) — How underserved is this by existing tools?
- **Trend Direction** (10%) — Is this pain growing or fading?

Scores above 8.5/10 are flagged as **🔥 Build this now.**

---

## Guardrails

- **No fabricated data.** If Reddit fetches fail, the skill says so and works only with real data.
- **Bias awareness.** Reddit skews toward certain demographics; the skill flags when a topic likely has Reddit blind spots.
- **Source citations.** Every major claim is attributed to a subreddit, post score, and approximate frequency.

---

## Contributing

Found a new mode that would be useful? Have a better output format for one of the existing modes? PRs welcome.

Suggested contributions:
- New mode specs in `references/modes.md`
- Subreddit discovery improvements for specific verticals
- Opportunity scoring calibration for specific market types

---

## License

MIT — use freely, attribution appreciated.

---

*Built as a custom Claude skill. Requires Claude with web access or the ability to make HTTP requests to Reddit's public JSON API (`reddit.com/*.json`).*

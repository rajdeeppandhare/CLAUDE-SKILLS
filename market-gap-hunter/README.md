# 🕵️ Market Gap Hunter

**A three-layer competitive intelligence skill for Claude.ai that finds underserved market segments — not just feature checklists.**

Most competitive analysis stops at "Competitor A is missing X feature." Market Gap Hunter goes two layers deeper: it finds the *buyers* everyone is ignoring and the *signals* competitors are actively avoiding talking about. Output is a ranked, scored opportunity brief ready to drive positioning decisions.

---

## 🎯 What It Does

| Most competitive analysis | Market Gap Hunter |
|---|---|
| Feature matrix (✓/✗) | Feature matrix + segment map + negative-space signals |
| "Competitor A misses X" | "These 3 buyer segments are systemically ignored across all competitors" |
| Flat list of weaknesses | Scored, ranked opportunity briefs with wedge theses |
| SWOT with extra steps | Evidence-backed gaps with graveyard checks and counter-risks |
| Generic output | Copy-paste-ready GTM positioning brief |

---

## 🧱 The Three Layers

### Layer 1 — Surface Gaps
Feature-level differences. Useful baseline, done fast. The skill never stops here.

### Layer 2 — Segment Gaps *(the real gold)*
Who is *everyone* ignoring? Analysis across four signal types:
- **Case study skew** — which buyer personas never appear in competitor marketing
- **Review use-case mining** — what users do with the product that was never marketed
- **Geography/regulatory gaps** — markets competitors implicitly excluded
- **Persona mismatch** — products that "technically work" for a segment they were never designed for

### Layer 3 — Negative-Space Gaps *(highest signal, most overlooked)*
What competitors actively avoid talking about:
- **Pricing page archaeology** — hidden tiers, "contact sales" walls, missing price floors
- **Changelog analysis** — what hasn't shipped in 12+ months (deprioritized or architecturally hard)
- **Job posting signals** — no ML hires = AI is bolted on; no SMB roles = SMB is ignored
- **1–3 star review mining** — cross-competitor pain patterns = systemic market gap, not one company's bug

---

## 📊 Gap Scoring

Every gap is scored before output. False positives are killed.

```
Gap Score = (Pain Severity × Segment Size) / Why-Nobody-Does-This Risk
```

Gaps below **4.0** are dropped. Gaps **7.0+** get full opportunity briefs. The graveyard check is mandatory for any top-scoring gap — because sometimes nobody does X because X was tried and it doesn't work.

---

## 📋 Example Output

```
GAP #1: Underserved SMB ops teams
Score: 8.4/10
Layer: Segment + Negative-Space

EVIDENCE
• A, B, C all have enterprise-only pricing tiers (≥$500/mo floor)
• 14 reviews across G2/Capterra mention "too complex for our 5-person team"
• None of A/B/C job postings include SMB-focused sales or CS roles
• Changelogs show no onboarding simplification shipped in 14+ months

WHY IT'S OPEN
Pricing and sales-motion lock-in, not a technical limitation. All three
incumbents built their GTM around enterprise AEs and high-touch onboarding.
Simplifying would require rebuilding their sales motion and pricing model.

WEDGE THESIS
A self-serve, $49/mo, SMB-first version targeting the "too complex" complaint
directly in G2 reviews — with a 14-day no-credit-card trial and opinionated
defaults that replace the configuration burden.

COUNTER-RISK
SMB usage-based pricing has a known CAC>LTV failure mode — verify by checking
unit economics of SMB-focused competitors in adjacent categories before committing.
```

---

## 🗂 Skill Package Contents

```
market-gap-hunter/
├── SKILL.md                          # Seven-phase pipeline (install this)
├── README.md                         # This file
└── references/
    ├── gap-frameworks.md             # Three-layer methodology with checklists
    ├── scoring-rubric.md             # Gap Score formula + graveyard heuristics
    └── output-template.md            # Ranked brief format (fill-in-the-blank)
```

---

## 🚀 Installation

**Claude.ai Projects (recommended):**
1. Open or create a Project in Claude.ai
2. Go to **Project Settings → Custom Instructions**
3. Paste the full contents of `SKILL.md`
4. Upload all files from `references/` to the Project knowledge base
5. Start a new conversation — Market Gap Hunter is ready

**One-off use:**
Paste `SKILL.md` directly into any Claude conversation before your first message.

---

## 💬 Trigger Phrases

```
"Run a competitive gap analysis on [A, B, C]"
"Who is nobody in [category] targeting?"
"Find the white space in [market]"
"What segments are all competitors ignoring?"
"Do a three-layer gap analysis for [product]"
"Map the negative space in [competitive set]"
```

---

## 📐 Design Principles

- **Evidence over inference** — every gap needs real sources (reviews, job posts, changelogs), not intuition
- **Segment over feature** — the most valuable gaps are *who*, not *what*
- **Scored, not catalogued** — the output is a ranked shortlist for decision-making, not a data dump
- **Graveyard-checked** — a gap only matters if it hasn't been tried and abandoned
- **Actionable by default** — every brief closes with a wedge thesis and a counter-risk verification step

---

## 🔗 More Skills

Part of the [CLAUDE-SKILLS](https://github.com/YOUR_USERNAME/CLAUDE-SKILLS) portfolio — production-quality skill packages for Claude.ai users.

Other skills in the portfolio:
- **Cybersecurity Excavator** — deep vulnerability analysis from code or architecture
- **SEO Excavator** — technical SEO audit and content gap analysis
- **Feature Architect** — structured feature scoping and PRD generation
- **Reddit Miner** — community intelligence from Reddit threads and comments
- **DB Architect** — database schema design from codebase or requirements
- **Job Copilot** — tailored job application and interview prep

---

## 📄 License

MIT — use freely, attribution appreciated.

Built for the Claude.ai non-developer community. No API key required, no code to run. Drop the skill into a Project and start asking questions.

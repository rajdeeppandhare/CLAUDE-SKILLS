# reverse-engineer-success

> Turn "I want X" into a falsifiable chain of metrics that bottoms out in **this week's action**.

---

## What it does

Most goal-setting produces abstract nouns — "strong distribution," "good marketing," "product-market fit." This skill forces every goal into a numbered causal chain where every level has a target number, a conversion rate or assumption, and a timeframe. The chain always terminates in something you can literally do in the next 7 days.

It also flags the **weakest link** — the assumption that's least proven and where effort should actually concentrate.

---

## When it triggers

Claude will use this skill whenever you:

- State a big outcome: *"I want $10M ARR"*, *"I want 100K subscribers"*, *"I want to raise a seed round"*, *"I want to get into [company/school]"*
- Ask to reverse-engineer, work backward from, or break down a goal
- Ask for a growth model or goal chain
- Have a vague success target that needs to become concrete

---

## Domains covered

| Domain | Example goal |
|--------|-------------|
| SaaS / B2B revenue | $10M ARR in 24 months |
| Consumer / B2C | $1M GMV by end of year |
| Audience / content | 100K YouTube subscribers |
| Career / hiring | Offer at a FAANG company in 6 months |
| Fitness | Run a sub-4hr marathon in 6 months |
| Fundraising | Close a $2M seed round |
| Product launch | 1,000 paying users at launch |

---

## Output structure

```
[GOAL + TIMEFRAME]
↓  [formula]
[Node — number]
↓  [rate/assumption — flagged if optimistic]
[Node — number]
↓  ...
↓  THIS WEEK
[Specific action, uncomfortable in its specificity]

⚠️  Weakest Link: [which assumption is least proven and why]
📊  Sensitivity: [what happens if the weakest rate is 2× off]
```

---

## Files

```
reverse-engineer-success/
├── SKILL.md                        # Core skill instructions
└── references/
    └── domain-templates.md         # Benchmarks + chain scaffolds per domain
```

---

## Install

Drop the `.skill` file into your Claude skills folder, or install via the Claude skills UI.

---

## Credits

Skill concept from the *reverse-engineer-success* prompt framework. Packaged as a Claude skill.

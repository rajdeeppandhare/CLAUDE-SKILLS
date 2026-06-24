---
name: reverse-engineer-success
description: Works backward from an ambitious goal (revenue, audience, career, fitness, etc.) to the concrete first action needed this week. Decomposes a vague outcome into a numbered causal chain — each step carrying a target, a conversion rate/assumption, and a timeframe — flags the weakest/least-proven link, and always terminates in something the user can literally do in the next 7 days, never another abstract goal. Use whenever the user states a big outcome they want ("I want $10M ARR", "I want 100K subscribers", "I want to get into [company/school]"), or explicitly asks to "reverse engineer" a goal, "work backward" from an outcome, or build a "growth model" / "goal chain."
---

# Reverse Engineer Success

Turns "I want X" into a falsifiable chain of metrics that bottoms out in this week's action — not another vague goal.

## The mistake this skill exists to prevent

Most goal-decomposition stops one level too early, at abstract nouns: "strong distribution," "good marketing," "product-market fit," "a solid brand." These feel like answers but aren't actionable — if a node in the chain can't be measured or done, it isn't a real link, it's a placeholder. Keep recursing past it.

## Process

1. **Anchor the goal.** Number + timeframe. "$10M ARR" → "$10M ARR within 24 months."
2. **Find the proximate driver.** The one metric that, if hit, guarantees the goal (e.g. ARR = #customers × ACV).
3. **Recurse backward, one level at a time.** Ask "what produces this number?" Every level must carry:
   - a target number
   - the rate/assumption connecting it to the level above
   - a timeframe, where it adds clarity
4. **Sanity-check every assumption.** Is a 20% trial→paid conversion realistic for this market? Flag anything optimistic, with a rough benchmark if you know one. Don't let an unrealistic rate quietly make the chain look easy.
5. **Stop only at "this week."** The bottom node must be something doable in the next 7 days. If a node is still a channel/strategy/category rather than an action, recurse one more level.
6. **Name the weakest link.** Call out which level in the chain is least proven or highest-risk — that's usually where effort should concentrate, not the top of the funnel.

## Output format

Top-down arrow chain, goal at the top, action at the bottom. Each arrow carries the rate/logic that connects the two numbers it sits between.

```
$10M ARR (24 months)
↓  #customers × $10K ACV
1,000 paying customers
↓  20% trial→paid (B2B SaaS benchmark ~15-25%, plausible)
5,000 trial signups
↓  5% landing page conversion
100,000 qualified leads
↓  2% CTR from content/outreach
5M impressions across chosen channel(s)
↓  THIS WEEK
Pick ONE channel. Ship 3 pieces of content / 20 cold outreaches. Measure CTR.
```

Follow the chain with 1–2 sentences naming the weakest link and why.

## Domain templates (not just SaaS)

- **Audience/content** — followers/subscribers ← avg views per post ← posts published × hook quality ← THIS WEEK: ship N pieces in the format already showing signal.
- **Career/hiring** — offer at target company ← final-round interviews ← screens passed ← applications/referrals sent ← THIS WEEK: shortlist 10 target roles, get 3 warm intros.
- **Fitness/physical goal** — target weight/lift ← weekly rate of change ← adherence % to plan ← THIS WEEK: fix the one habit that's currently the bottleneck (e.g. log every meal, hit 3 sessions).
- **Fundraising** — round closed at $X ← term sheets ← partner meetings ← investor intros ← THIS WEEK: build target list of 30, get 5 warm intros.

Use these as scaffolds, not scripts — derive the actual numbers/rates for the user's specific situation rather than reusing the example verbatim.

## Failure modes to catch in your own output before showing it

- **Circular node:** a step that's just a synonym of the one above it (e.g. "distribution" → "acquisition strategy"). That's not causal, re-derive it.
- **No number attached:** anything unmeasurable can't be reverse-engineered from. Force a number even if it's a rough estimate.
- **Unflagged assumption:** every conversion rate gets a quick plausibility check, not a free pass.
- **Stopping above "this week":** abstractions like "build a brand" or "improve our funnel" are not bottom nodes — keep going until it's a verb a person does on a specific day.

# content-craft

A Claude Skill that turns thin content requests ("write a caption", "promote our product", "make a tagline") into premium, emotionally-specific copy — by filling in missing context first, then routing to the right writing framework for the format.

Built on one core insight: **the model isn't usually the bottleneck, the brief is.** "Promote our coffee shop" and "You're a senior brand strategist, create premium emotionally-driven taglines for a specialty coffee brand targeting remote workers 25-40, personality is warm/ambitious/minimalist, focus on belonging and daily ritual" produce wildly different output quality from the same model. This skill bakes that context-filling step into the workflow instead of relying on the user to know to write it that way.

## What it covers

Not marketing-only — any short-to-medium content format:

- Taglines / slogans
- Social captions
- Video / ad scripts
- Blog & landing page hooks
- Email subject lines
- LinkedIn / thought-leadership posts
- Founder stories / About pages
- Minimalist "Apple/Nike-style" voice rewrites

## How it works

1. **Fill the gap (BAEO)** — Brand/Subject, Audience, Emotion, Outcome. Asks if 2+ are missing on a non-trivial request; infers and states assumptions on casual ones.
2. **Route to a framework** — each format has its own structural pattern (e.g. captions: Hook → Emotion → Benefit → CTA), detailed in `references/frameworks.md`.
3. **Quality bar** — actively avoids generic-AI tells ("take your business to the next level," "experience the difference") and favors concrete detail over abstract claims.
4. **Deliver variants** — 3-5 angled options (e.g. clear / bold / premium) instead of one generic answer, or a clear before→after when punching up existing copy.

## Structure

```
content-craft/
├── SKILL.md                  # triggering + workflow Claude follows
└── references/
    ├── frameworks.md         # the 8 content-format formulas + worked examples
    ├── voice-styles.md       # tone calibration (premium / warm / bold / luxury / playful)
    └── examples.md           # full before→after across coffee / SaaS / fitness verticals
```

## Install

Drop the `content-craft/` folder (or unzip `content-craft.skill`) wherever Claude loads skills from. No dependencies, no scripts to run — it's pure instructions + reference docs.

## Trigger examples

- "Write a caption for our new product launch"
- "Give me 10 taglines for a coffee brand"
- "Make this sound more premium / more Apple-like"
- "Write our founder story for the About page"
- *(pastes rough draft)* "punch this up"

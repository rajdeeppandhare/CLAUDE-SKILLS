# UI Architect Skill

A composition-first premium frontend builder for Claude. Produces production-grade React/HTML UI that looks like it shipped from a funded startup — not a tutorial.

## What it does

When loaded, this skill replaces the default "blank canvas" approach with a structured pipeline:

1. **Classify** — determines what's being built and whether to output an in-chat Artifact or a full scaffolded project
2. **Source** — walks a decision tree (shadcn primitives → curated patterns → live lookup → custom) before writing anything from scratch
3. **Tokenize** — locks in a consistent spacing/type/color/motion system derived from premium references (Stripe, Linear, Vercel)
4. **Assemble** — composes sourced patterns around the user's real content and brand
5. **QA** — runs a self-critique checklist before shipping

## When to use it

- Landing pages, marketing sites
- SaaS dashboards and app shells
- Admin panels, data tables
- Pricing pages, auth flows
- Trading/finance dashboards
- Any React component or page where "looks credible and funded" matters more than "looks unique"

For something genuinely experimental or one-of-a-kind, use the built-in `frontend-design` skill instead — it's designed for bespoke identity over proven patterns.

## File structure

```
ui-architect/
├── SKILL.md                          ← Main skill instructions (load this)
├── README.md                         ← This file
├── ui-architect.skill                ← Skill metadata
└── references/
    ├── component-decision-tree.md    ← Phase 2: sourcing logic
    ├── design-system.md              ← Phase 3: tokens, spacing, color, motion
    ├── marketing-components.md       ← Curated landing page / marketing patterns
    ├── app-components.md             ← Curated dashboard / app shell patterns
    └── live-lookup-and-qa.md         ← Live lookup protocol + QA checklist
```

## How to install

Upload `SKILL.md` (or the full folder) into your Claude project. Claude will detect it and apply the pipeline automatically on any UI request.

## Key constraints (Artifact mode)

- No Framer Motion — use CSS transitions/keyframes instead
- shadcn/ui primitives available via `@/components/ui/*` with no install
- Tailwind has no compiler — no arbitrary values like `bg-[#hex]`, use CSS variables
- Single `.jsx` output file, no `package.json`

Full project mode lifts all of these and supports real `npm install`.

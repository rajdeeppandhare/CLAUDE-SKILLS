# Marketing Components — Curated Patterns

Compositions for landing pages, marketing sites, and public-facing pages.
Adapt to the user's real content — never ship reference copy verbatim.

---

## Navbar

**Pattern: sticky top bar, logo left, links center, CTA right**

```jsx
// Adapt: replace brand name, links, and CTA label
<nav style={{
  position: 'sticky', top: 0, zIndex: 50,
  display: 'flex', alignItems: 'center', justifyContent: 'space-between',
  padding: '0 var(--space-4)', height: 64,
  background: 'rgba(255,255,255,0.85)',
  backdropFilter: 'blur(12px)',
  borderBottom: '1px solid var(--border)',
}}>
  <span style={{ fontWeight: 700, fontSize: 18 }}>BrandName</span>
  <div style={{ display: 'flex', gap: 'var(--space-3)', fontSize: 14 }}>
    <a href="#">Product</a>
    <a href="#">Pricing</a>
    <a href="#">Docs</a>
  </div>
  <Button size="sm">Get started</Button>
</nav>
```

Dark variant: swap `rgba(255,255,255,0.85)` → `rgba(15,15,16,0.85)` and `var(--border)` → dark border.

---

## Hero

**Pattern A: centered, eyebrow label + headline + sub + CTA pair**

Best for: first-impression SaaS, single strong value prop.

```
[Eyebrow badge]
[H1 headline — 2 lines max]
[Body subtitle — 1–2 sentences]
[Primary CTA]  [Secondary CTA / demo link]
[Social proof: "Trusted by N teams at ..."]
```

Key rules:
- Headline max 8 words if possible. Never "Supercharge your workflow."
- Eyebrow badge uses `Badge` (outline variant), 11px uppercase, brand accent color
- CTA pair: one filled (primary action), one ghost (secondary / "see demo")
- Social proof sits below CTA, muted color, small logos or avatar stack

**Pattern B: split layout, copy left + product screenshot right**

Best for: when there's a real screenshot or UI preview to show.

```
[Left col — 50%]                [Right col — 50%]
  Eyebrow                         [Product screenshot / mockup]
  H1                              [in a browser chrome or device frame]
  Subtitle
  CTAs
  Social proof
```

Screenshot treatment: wrap in a subtle browser-chrome div (three dots + URL bar), add `--shadow-xl`, 8px border-radius.

---

## Feature grid

**Pattern: 3-column icon + title + description cards**

```jsx
const features = [
  { icon: <Zap />, title: "Fast by default", description: "Optimised for..." },
  // ...
]

<section style={{ padding: 'var(--space-12) var(--space-4)' }}>
  <h2 style={{ textAlign: 'center', marginBottom: 'var(--space-6)' }}>
    Everything you need
  </h2>
  <div style={{
    display: 'grid', gridTemplateColumns: 'repeat(3, 1fr)',
    gap: 'var(--space-3)',
  }}>
    {features.map(f => (
      <Card key={f.title}>
        <CardHeader>
          <div style={{ color: 'var(--accent)', marginBottom: 'var(--space-1)' }}>
            {f.icon}
          </div>
          <h3 style={{ fontWeight: 600, fontSize: 16 }}>{f.title}</h3>
        </CardHeader>
        <CardContent>
          <p style={{ color: 'var(--text-secondary)', fontSize: 14 }}>{f.description}</p>
        </CardContent>
      </Card>
    ))}
  </div>
</section>
```

Anti-pattern: do NOT add numbered badges (01/02/03) unless the content is genuinely ordered steps.

---

## Pricing table

**Pattern: 3 tiers, middle tier highlighted**

Tiers: Starter (free or low) / Pro (highlighted) / Enterprise (custom)

```
[Starter]          [PRO — highlighted ring]     [Enterprise]
$0/mo              $29/mo                        Custom
Feature list       Feature list                  Feature list
[Get started]      [Start free trial]            [Talk to sales]
```

Implementation notes:
- Highlighted tier: `border: 2px solid var(--accent)`, `--shadow-lg`, slightly taller card
- Use `Badge` ("Most popular") pinned to top-center of highlighted card
- Checkmark list: lucide `Check` icon in accent color, `X` icon in muted for unavailable
- Annual/monthly toggle: `Switch` component above the table, updates prices

---

## Testimonials

**Pattern A: quote grid (3 cards)**
```
[Avatar + Name + Company]
"Quote text — specific, outcome-focused, 2–3 sentences max"
```

**Pattern B: large single quote (marquee/featured)**
```
[Large quotation mark in accent color]
"Single impactful quote"
— Name, Title at Company
[Company logo]
```

Anti-pattern: do not use generic quotes like "This tool changed my life." Use outcome-specific language ("Cut our deploy time from 45 minutes to 3").

---

## CTA section

**Pattern: full-width band above footer**

```jsx
<section style={{
  background: 'var(--accent)', color: '#fff',
  padding: 'var(--space-12) var(--space-4)',
  textAlign: 'center',
}}>
  <h2 style={{ fontSize: 36, fontWeight: 700, marginBottom: 'var(--space-2)' }}>
    Ready to ship faster?
  </h2>
  <p style={{ opacity: 0.85, marginBottom: 'var(--space-4)' }}>
    Join N teams already using BrandName.
  </p>
  <Button variant="outline" style={{ color: '#fff', borderColor: 'rgba(255,255,255,0.5)' }}>
    Start for free
  </Button>
</section>
```

---

## Footer

**Pattern: 4-column link grid + bottom bar**

```
[Logo + tagline]   [Product links]   [Company links]   [Legal links]
─────────────────────────────────────────────────────────────────────
© 2025 BrandName · Privacy · Terms · Status
```

- Top section: `display: grid; grid-template-columns: 2fr 1fr 1fr 1fr`
- Bottom bar: `border-top: 1px solid var(--border)`, `color: var(--text-tertiary)`, `font-size: 13px`
- Include a social icon row (lucide doesn't have brand icons — use a simple text list or SVG inline)

---

## Logo cloud / social proof strip

```jsx
<section style={{
  padding: 'var(--space-4) var(--space-4)',
  borderTop: '1px solid var(--border)',
  borderBottom: '1px solid var(--border)',
}}>
  <p style={{ textAlign: 'center', color: 'var(--text-tertiary)', fontSize: 13, marginBottom: 'var(--space-3)' }}>
    Trusted by teams at
  </p>
  <div style={{ display: 'flex', justifyContent: 'center', alignItems: 'center', gap: 'var(--space-6)', opacity: 0.5 }}>
    {/* Company name wordmarks in muted color — do not use real logos without permission */}
    {['Acme Corp', 'Globex', 'Initech', 'Umbrella', 'Hooli'].map(name => (
      <span key={name} style={{ fontWeight: 700, fontSize: 15, letterSpacing: '-0.01em' }}>{name}</span>
    ))}
  </div>
</section>
```

---

## Section spacing rules

| Position | Top padding | Bottom padding |
|---|---|---|
| First section after hero | `var(--space-12)` | `var(--space-12)` |
| Mid-page sections | `var(--space-8)` | `var(--space-8)` |
| CTA section | `var(--space-12)` | `var(--space-12)` |
| Footer | `var(--space-6)` | `var(--space-6)` |

Consistent section rhythm is one of the highest-leverage things separating "polished" from "tutorial."

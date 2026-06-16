# Design System — Tokens, Spacing, Type, Color, Motion

Apply this file at the start of Phase 3 (Tokenize). Lock in all four systems before writing component code.

---

## Spacing scale (8px base)

All spacing in multiples of 8px. Never use arbitrary pixel values.

| Token | Value | Use |
|---|---|---|
| `space-1` | 8px | Tight inline gaps (icon + label, badge padding) |
| `space-2` | 16px | Component internal padding, small gaps |
| `space-3` | 24px | Card padding, section internal gaps |
| `space-4` | 32px | Between related sections, generous card padding |
| `space-6` | 48px | Between major page sections |
| `space-8` | 64px | Large section breaks, hero vertical padding |
| `space-12` | 96px | Top-of-page hero, footer breathing room |
| `space-16` | 128px | Full-bleed section padding on marketing pages |

In Artifact mode (no Tailwind compiler), express these as CSS variables or inline style values, not as `p-[48px]`.

```css
:root {
  --space-1: 8px;
  --space-2: 16px;
  --space-3: 24px;
  --space-4: 32px;
  --space-6: 48px;
  --space-8: 64px;
  --space-12: 96px;
  --space-16: 128px;
}
```

---

## Typography

Pick one pairing per build and stick to it. Never mix more than two typefaces.

### Pairing A — Stripe-neutral (default for SaaS/neutral products)
- Heading: `Inter`, weight 600–700
- Body: `Inter`, weight 400–500
- Mono: `JetBrains Mono` or `Fira Code` (for code snippets only)

### Pairing B — Linear-dark (preferred for dev tools, dark-mode-first products)
- Heading: `Inter`, weight 700, letter-spacing -0.02em
- Body: `Inter`, weight 400, color slightly muted (`--text-secondary`)
- Mono: `JetBrains Mono`

### Pairing C — Vercel-mono (editorial, high-contrast, minimal)
- Heading: `Geist` or `Inter`, weight 800, uppercase tracking for small labels
- Body: `Inter`, weight 400
- Accent labels: uppercase, `letter-spacing: 0.08em`, `font-size: 11px`

### Type scale

| Role | Size | Weight | Line-height |
|---|---|---|---|
| Display / hero | 48–64px | 700–800 | 1.1 |
| H1 | 36px | 700 | 1.2 |
| H2 | 28px | 600 | 1.25 |
| H3 | 20px | 600 | 1.3 |
| Body large | 18px | 400 | 1.6 |
| Body default | 16px | 400 | 1.5 |
| Body small | 14px | 400 | 1.5 |
| Label / caption | 12px | 500 | 1.4 |
| Overline | 11px | 600 | 1.4, uppercase, +0.08em tracking |

---

## Color palettes

Pick one direction. Derive the full palette from it. Do not combine two directions.

### Direction 1 — Stripe-neutral (light mode, high trust)
```css
:root {
  --bg-base: #ffffff;
  --bg-subtle: #f6f8fa;
  --bg-muted: #eef0f3;
  --border: #e2e5ea;
  --text-primary: #0f1117;
  --text-secondary: #5c6370;
  --text-tertiary: #9ba3af;
  --accent: #635bff;          /* Stripe indigo */
  --accent-hover: #4f46e5;
  --accent-subtle: #ede9fe;
  --success: #00c48c;
  --warning: #f59e0b;
  --danger: #ef4444;
}
```

### Direction 2 — Linear-dark (dark mode, developer-first)
```css
:root {
  --bg-base: #0f0f10;
  --bg-subtle: #1a1a1c;
  --bg-muted: #222224;
  --border: #2e2e32;
  --text-primary: #f0f0f2;
  --text-secondary: #8b8b91;
  --text-tertiary: #55555a;
  --accent: #5e6ad2;          /* Linear purple-blue */
  --accent-hover: #7c85e0;
  --accent-subtle: #1e1f3a;
  --success: #16a34a;
  --warning: #d97706;
  --danger: #dc2626;
}
```

### Direction 3 — Vercel-mono (high-contrast, minimal)
```css
:root {
  --bg-base: #000000;
  --bg-subtle: #111111;
  --bg-muted: #1a1a1a;
  --border: #333333;
  --text-primary: #ffffff;
  --text-secondary: #888888;
  --text-tertiary: #555555;
  --accent: #ffffff;           /* Mono: white is the accent */
  --accent-hover: #e5e5e5;
  --accent-subtle: #1a1a1a;
  --success: #22c55e;
  --warning: #eab308;
  --danger: #ef4444;
}
```

### Direction 4 — Financial/dark (trading dashboards, data-heavy)
```css
:root {
  --bg-base: #0b0e14;
  --bg-subtle: #131720;
  --bg-muted: #1c2230;
  --border: #252d3d;
  --text-primary: #e2e8f0;
  --text-secondary: #8892a4;
  --text-tertiary: #4a5568;
  --accent: #3b82f6;           /* Clear blue for data callouts */
  --accent-hover: #60a5fa;
  --accent-subtle: #1e3a5f;
  --positive: #22c55e;         /* Green = gain */
  --negative: #ef4444;         /* Red = loss */
  --warning: #f59e0b;
}
```

---

## Motion

### Artifact mode (CSS only — no Framer Motion)

```css
/* Standard easing tokens */
:root {
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1); /* subtle spring */
  --duration-fast: 120ms;
  --duration-base: 200ms;
  --duration-slow: 350ms;
}

/* Hover lift pattern */
.card-interactive {
  transition: transform var(--duration-base) var(--ease-out),
              box-shadow var(--duration-base) var(--ease-out);
}
.card-interactive:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.12);
}

/* Fade-in on mount (keyframe) */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
.animate-fade-up {
  animation: fadeUp var(--duration-slow) var(--ease-out) both;
}

/* Stagger children */
.stagger > *:nth-child(1) { animation-delay: 0ms; }
.stagger > *:nth-child(2) { animation-delay: 60ms; }
.stagger > *:nth-child(3) { animation-delay: 120ms; }
.stagger > *:nth-child(4) { animation-delay: 180ms; }
```

**One accessory rule:** apply an entrance animation to one section (usually the hero or first data panel). Everything else uses hover transitions only. Do not animate every card.

### Full project mode (Framer Motion available)

```jsx
// Standard page-entry variant
const fadeUp = {
  hidden: { opacity: 0, y: 16 },
  visible: { opacity: 1, y: 0, transition: { duration: 0.45, ease: [0.16, 1, 0.3, 1] } }
}

// Stagger container
const staggerContainer = {
  hidden: {},
  visible: { transition: { staggerChildren: 0.07 } }
}

// Usage
<motion.div variants={staggerContainer} initial="hidden" animate="visible">
  {items.map(item => (
    <motion.div key={item.id} variants={fadeUp}>{/* ... */}</motion.div>
  ))}
</motion.div>
```

---

## Shadow scale

```css
:root {
  --shadow-xs:  0 1px 2px rgba(0,0,0,0.05);
  --shadow-sm:  0 2px 6px rgba(0,0,0,0.08);
  --shadow-md:  0 4px 16px rgba(0,0,0,0.10);
  --shadow-lg:  0 8px 32px rgba(0,0,0,0.14);
  --shadow-xl:  0 16px 56px rgba(0,0,0,0.18);
}
```

For dark themes, increase alpha: `rgba(0,0,0,0.30)` on `--shadow-md` and above.

---

## Border radius scale

```css
:root {
  --radius-sm:  4px;   /* badges, tags, small chips */
  --radius-md:  8px;   /* inputs, buttons, small cards */
  --radius-lg:  12px;  /* cards, panels */
  --radius-xl:  16px;  /* modal dialogs, large cards */
  --radius-2xl: 24px;  /* hero containers, feature callout boxes */
  --radius-full: 9999px; /* pills, avatars */
}
```

Pick one radius "tier" per build and stay consistent. Mixing sm/md/lg/xl on the same page reads as unfinished.

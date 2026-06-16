# Component Decision Tree

Run this tree top-to-bottom for **every** UI element before writing custom code.
Stop at the first step that has a fit. Never skip steps.

---

## Step 1 — shadcn/ui primitive

Check this list first. If the element maps to one of these, use it directly.

| Need | shadcn component |
|---|---|
| Button, link button, icon button | `Button` (variant: default / outline / ghost / destructive) |
| Text input, textarea, search | `Input`, `Textarea` |
| Dropdown select | `Select` + `SelectItem` |
| Modal / dialog | `Dialog` |
| Slide-in panel | `Sheet` |
| Tabs | `Tabs` + `TabsList` + `TabsTrigger` + `TabsContent` |
| Card container | `Card` + `CardHeader` + `CardContent` + `CardFooter` |
| Data table | `Table` + `TableHeader` + `TableRow` + `TableCell` |
| Status pill / label | `Badge` (variant: default / secondary / outline / destructive) |
| Command palette / search | `Command` + `CommandInput` + `CommandList` |
| Tooltip | `Tooltip` + `TooltipTrigger` + `TooltipContent` |
| Checkbox | `Checkbox` |
| Radio group | `RadioGroup` + `RadioGroupItem` |
| Toggle switch | `Switch` |
| Slider | `Slider` |
| Skeleton loader | `Skeleton` |
| Avatar | `Avatar` + `AvatarImage` + `AvatarFallback` |
| Separator / divider | `Separator` |
| Accordion | `Accordion` + `AccordionItem` |
| Alert / banner | `Alert` + `AlertTitle` + `AlertDescription` |
| Progress bar | `Progress` |
| Popover | `Popover` + `PopoverTrigger` + `PopoverContent` |
| Calendar / date picker | `Calendar` |
| Form field wrapper | `Form` + `FormField` + `FormLabel` + `FormMessage` |

Import pattern (Artifact mode):
```jsx
import { Button } from "@/components/ui/button"
import { Card, CardHeader, CardContent } from "@/components/ui/card"
// etc.
```

**If the element is on this list, use the shadcn component. Do not build a custom version.**

---

## Step 2 — Curated pattern library

If Step 1 has no fit (the element is a composition, not a primitive), check the curated files:

- **Landing page sections** (hero, navbar, features grid, pricing table, testimonials, CTA, footer) → `marketing-components.md`
- **App UI** (dashboard shell, sidebar nav, stat cards, data tables with filters, empty states, loading skeletons, notification feed, financial chart layouts) → `app-components.md`

These files contain full, copy-adaptable compositions. Adapt the pattern to the user's content — don't ship the reference copy verbatim.

---

## Step 3 — Live lookup

Only reach this step if:
- The brief calls for something not covered by shadcn primitives or the curated library
- The user explicitly asks for a novel/current pattern
- `web_search` / `web_fetch` are available in this runtime context

If you reach this step, follow the protocol in `live-lookup-and-qa.md` exactly.

Do **not** use live lookup as a shortcut to skip Steps 1 and 2.

---

## Step 4 — Custom build

Last resort. Even here, build out of Step 1 primitives wherever possible.

Reserve fully custom code for exactly one signature element per build — the thing the brief genuinely needs that nothing else covers (an unusual chart type, a brand-specific animation, a layout the curated library doesn't have).

When you write custom code, document why Steps 1–3 didn't apply.

---

## What counts as a "primitive" vs a "composition"?

**Primitive** — a single interactive or display unit that shadcn ships as a standalone component (Button, Input, Badge, Skeleton, Dialog…). Use it directly from shadcn.

**Composition** — multiple primitives assembled into a section or feature (a pricing card row, a dashboard sidebar with nav items and a user footer, a stat card grid with sparklines). Check the curated library first; build custom only if nothing fits.

When in doubt: if it has more than two moving parts, it's a composition. Go to Step 2.

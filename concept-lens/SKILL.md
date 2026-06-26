---
name: concept-lens
description: >-
  A universal concept translation engine that re-explains any idea through the user's own world, not just one analogy, but the full range: domain analogies (gaming, cooking, cricket, engineering, medicine, finance, music, army, space, etc.), narrative modes (Story, Detective, Adventure, Movie, Escape Room, Lego, Coaching), and cross-domain reasoning. Generates 5-10 differently-framed translations and lets the user pick what clicks, then builds an in-session Analogy Profile so later explanations route through their best-fit mental models. Use whenever the user wants an analogy, says "explain it like I'm a [X]", "explain using [domain]", "I still don't get it", "give me a different way to think about this", "ELI5", or asks for a concept from scratch. Also use proactively whenever an explanation would benefit from concrete grounding, even unasked.
---

# ConceptLens

*Every concept, explained through your world.*

A universal translation engine that converts abstract ideas into mental models the user already owns — not one analogy, but a full tournament of framings until something clicks, then a personalized profile that routes every future explanation automatically.

---

## The core problem this solves

Every explanation assumes a mental model the listener may not have. ConceptLens doesn't change the learner — it changes the explanation. Same concept, different lens, different person.

---

## Three layers of translation

### Layer 1 — Domain Analogies
Map the concept onto a world the user already understands deeply. See `references/domain-library.md` for the full library of 20+ domains with worked examples.

### Layer 2 — Narrative Modes
Re-frame the concept as a different *type of experience* — not just a different subject, but a different cognitive register. See `references/narrative-modes.md` for all 8 modes with templates.

### Layer 3 — Cross-Domain Reasoning
Connect concepts *across* domains to build intuition and creativity. The most powerful layer — when a user can explain gradient descent via cricket AND chess AND biology, they've internalized it.

---

## Process

### Step 1 — Read the user's world
Before translating, scan for signals:
- What domain vocabulary do they use naturally? (sports terms, cooking terms, code, business jargon)
- What examples or metaphors did *they* use in their question?
- Have they mentioned a profession, hobby, or interest anywhere in the conversation?
- Any in-session Analogy Profile built from prior turns?

If no signals: default to **Analogy Tournament** (generate 5 translations across varied domains and let the user pick).

### Step 2 — Choose the mode

| Situation | What to do |
|-----------|-----------|
| User gives a clear domain cue ("I'm a chef", "explain like I'm a gamer") | Go deep on that one domain — primary analogy + 2 variations |
| User says "give me different ways" / "make it click" | Run Analogy Tournament: 5–8 domains, one crisp paragraph each |
| User says "explain it as a [story / detective / adventure / etc.]" | Activate the named Narrative Mode |
| User says "explain X using Y" (cross-domain) | Cross-Domain Reasoning mode |
| Concept is highly abstract and needs grounding | Default to Tournament, lead with the two most universal domains (cooking, sports) |

### Step 3 — Deliver the translation

For **single-domain** delivery:
- Open with the analogy (2–4 sentences, vivid and specific)
- Map each key component of the concept to its analogy counterpart explicitly
- Close with the real terminology, now anchored to the analogy

For **Analogy Tournament** delivery:
- Lead with: "Here are [N] ways to think about [concept] — pick the one that clicks:"
- Each entry: emoji + domain label + 3–5 sentence analogy, crisp and concrete
- End with: "Which framing felt most natural? I'll use that lens going forward."

For **Narrative Mode** delivery:
- Use the mode's template from `references/narrative-modes.md`
- Stay in the mode's register throughout (don't break the detective voice mid-explanation)

### Step 4 — Update the Analogy Profile

After the user responds (picks a framing, says "that clicked", asks a follow-up in domain terms, etc.), mentally update the in-session profile:

```
Analogy Profile (in-session)
✓ Works well: [domains that landed]
✗ Didn't click: [domains the user ignored or rejected]
→ Route next explanations through: [top 1-2 domains]
```

Reference this profile silently in subsequent explanations — don't announce it, just use it.

---

## Analogy Tournament format

```
Here are [N] ways to think about [CONCEPT]:

🎮 **Gaming** — [3-5 sentence analogy]

🍳 **Cooking** — [3-5 sentence analogy]

⚽ **Football** — [3-5 sentence analogy]

🏗 **Construction** — [3-5 sentence analogy]

🎼 **Orchestra** — [3-5 sentence analogy]

---
Which one clicked? I'll use that lens for everything we cover next.
```

Minimum 5 entries for a tournament. Maximum 10. Pick domains that are genuinely varied in cognitive style — don't run 3 sports analogies.

---

## Cross-Domain Reasoning format

When the user asks "explain X using Y" (e.g., "explain Kubernetes using a beehive"):

1. Honour the request — build the analogy in Y first, in full
2. Map every major component of X to its Y counterpart explicitly:
   ```
   X component → Y equivalent (why this mapping works)
   ```
3. Name where the analogy breaks down (this is important — false precision is worse than no analogy)
4. Offer an Analogy Evolution: "Want to go further? We can translate this to [Z] and then back to the original — that's where real intuition builds."

---

## Analogy Evolution (chaining)

When a user wants to go deep, chain analogies across domains:

```
[CONCEPT]
    ↓ explained as [Domain A]
    ↓ Domain A compared to [Domain B]
    ↓ Domain B mapped back to [CONCEPT]
    → User now sees the concept through multiple lenses simultaneously
```

Example:
```
Neural Networks
    ↓ explained as a cricket batsman adjusting after each delivery
    ↓ batsman learning compared to a chef refining a recipe through taste tests
    ↓ chef's iterative refinement mapped back to gradient descent
    → User now has 3 anchored mental models for the same process
```

Offer this when the user wants to go beyond "I understand it" to "I really *get* it."

---

## Failure modes to avoid

- **Generic analogy** — "it's like a brain but for computers." Too vague. Every analogy must map specific components to specific counterparts.
- **Broken analogy** — using an analogy that fails at the most important part of the concept (e.g., comparing neural networks to a flowchart, which implies fixed logic — the opposite of what makes them powerful). Always name where the analogy breaks.
- **Analogy without mapping** — telling the story without explicitly connecting it back to the concept's real terms. Always close the loop.
- **One analogy per concept** — the whole point of ConceptLens is that different people need different lenses. If something didn't click, immediately offer a completely different domain.
- **Switching register mid-mode** — if you start Detective Mode, stay in it. Don't drop the voice after one paragraph.

---

## Reference files

- [`references/domain-library.md`](references/domain-library.md) — 20+ domain analogy libraries with worked examples across common technical/abstract concepts
- [`references/narrative-modes.md`](references/narrative-modes.md) — 8 narrative mode templates (Story, Detective, Adventure/RPG, Movie, Escape Room, Lego, Cooking Recipe, Coaching)

# Narrative Modes

Eight ways to re-frame a concept as a different *type of experience* — not a different subject area (that's `domain-library.md`), but a different cognitive register. The same concept told as a Story feels completely different from the same concept told as a Detective case, even with zero domain-swapping.

Use exactly one mode per response and stay in its register the whole way through — switching registers mid-explanation is one of ConceptLens's named failure modes.

---

## 1. Story Mode
**Register:** A character faces a problem, and the concept *is* the thing that resolves it. Emotional stakes, simple arc (setup → complication → resolution).

**Template:**
```
[Character] needed to [goal]. The problem was [obstacle that mirrors the concept's core tension].
They tried [naive approach] — but [why it failed, mapped to a real failure mode of the concept].
Then they realized: [the concept's key insight], so they [action mapped to the technique].
Now [character] could [resolved goal] — and that's exactly what [real term] does.
```

**When to use:** Concepts with a clear "tension → resolution" shape (e.g., explaining caching: "a librarian who got tired of walking to the back stacks for the same five books every day").

---

## 2. Detective Mode
**Register:** Investigative, deductive, building toward a reveal. Treat the concept's mechanism as "whodunit" — clues are gathered, false leads eliminated, and the real cause is revealed.

**Template:**
```
The case: [symptom/observed problem].
Clue #1: [observation that hints at part of the mechanism].
Clue #2: [observation that hints at another part].
A tempting but wrong suspect: [a plausible-but-incorrect explanation, and why it doesn't hold up — mirrors a common misconception].
The real culprit: [the actual mechanism], and here's how it explains every clue: [tie clues back].
Case closed — in real terms, that's [actual concept/terminology].
```

**When to use:** Debugging, root-cause concepts, anything where the "obvious" explanation is a known misconception worth debunking (overfitting, race conditions, confounding variables).

---

## 3. Adventure / RPG Mode
**Register:** Quest structure — stats, levels, items, a final boss. The user (or a hero) progresses through stages, gaining "abilities" that map to sub-concepts.

**Template:**
```
🗺️ Quest: [concept]

Level 1 — [sub-concept A]: You gain the ability to [what understanding this unlocks].
Level 2 — [sub-concept B]: New gear: [metaphorical item] lets you [capability].
⚠️ Boss fight — [the hardest/most counterintuitive part of the concept]: this boss is tough because [why it's hard], and you beat it by [the actual resolution/technique].
🏆 Victory: you've now mastered [concept], i.e. [real terminology, tied back explicitly].
```

**When to use:** Multi-step concepts that build hierarchically (a pipeline, a layered architecture, a multi-stage algorithm) — the leveling structure mirrors genuine prerequisite structure.

---

## 4. Movie Mode
**Register:** Cinematic, scene-based, with a "director's commentary" aside that breaks down technique. Good for concepts with a strong before/after or transformation arc.

**Template:**
```
🎬 Scene: [opening situation, set up like a film's first scene]

[Describe the unfolding "action" of the concept as if narrating a movie scene — use present tense, vivid, short sentences]

🎙️ Director's commentary: "Notice how [moment in the scene] — that's actually [real concept term]. We staged it this way because [why the real mechanism behaves like that]."

Cut to: [the resolution scene, mapped to the concept's outcome]
```

**When to use:** Concepts with a vivid transformation (data transformation pipelines, before/after optimization, state changes) — the "director's commentary" aside is the explicit mapping-back step.

---

## 5. Escape Room Mode
**Register:** Puzzle-solving under constraint. The concept is reframed as a sequence of locks that must be opened in the right order, each requiring a piece of understanding from the last.

**Template:**
```
🔒 You're locked in a room. To get out, you need to solve [N] locks in order.

Lock 1: [first sub-problem]. The key: [insight that unlocks it].
Lock 2: [second sub-problem, which only makes sense once Lock 1 is solved]. The key: [insight].
Lock 3 (the tricky one): [the counterintuitive part]. Most people get stuck here because [common error]. The actual key: [correct insight].

🚪 Door opens: you've just solved [concept] — in real terms, [terminology], and the order you solved the locks in is the order [concept]'s steps actually happen.
```

**When to use:** Concepts where order/sequencing matters and a common mistake is solving steps out of order or skipping a prerequisite (algorithms with strict dependencies, proofs, multi-step setup processes).

---

## 6. Lego Mode
**Register:** Physical, modular, snap-together construction. Best for concepts that are fundamentally about composing small reusable pieces into something bigger.

**Template:**
```
🧱 Think of [concept] as building with Lego.

Brick type A = [sub-component A] — on its own it does [limited thing].
Brick type B = [sub-component B] — snaps onto A because [why these two compose].
When you snap enough of these together in [pattern], you get [the emergent capability of the full concept].

The instruction manual (the rule for how bricks must connect) = [the actual constraint/rule in the real concept].
If you ignore the manual and force pieces together wrong, you get [what failure/bug looks like].
```

**When to use:** Compositional concepts — function composition, modular architecture, building block patterns (CSS box model, API composition, molecular bonding).

---

## 7. Cooking Recipe Mode
**Register:** Literally a recipe card — ingredients list, numbered steps, a "chef's note" for the tricky part. (Distinct from the *Cooking domain analogy* in `domain-library.md` — this is a structural/narrative format, usable even to explain a non-food concept by structuring it like a recipe.)

**Template:**
```
📋 Recipe: [Concept]

Ingredients:
- [sub-component 1]
- [sub-component 2]
- [sub-component 3]

Steps:
1. [first step, mapped to the concept's first operation]
2. [second step — note any step that must happen before another, mirroring real dependencies]
3. [final step — the "plating," i.e., the concept's output]

👨‍🍳 Chef's note: [the part most people get wrong, and the actual technique that fixes it]
```

**When to use:** Sequential processes with discrete steps and a clear "yield" (build pipelines, statistical procedures, setup/configuration workflows).

---

## 8. Coaching Mode
**Register:** A coach talking directly to the user (second person), focused on practical mastery rather than theory — "here's what you're actually trying to do, here's the mistake everyone makes, here's the fix."

**Template:**
```
Alright, let's break down [concept] like I'm coaching you through it.

Here's the goal: [what you're actually trying to achieve, in plain terms].
Here's the mistake almost everyone makes early on: [common misconception or naive approach].
Here's the fix: [the actual correct technique/mental model].
Drill it like this: [a small mental exercise or way to check your own understanding going forward].

That's it. In the jargon, what you just learned is called [real terminology] — but the way I just described it is what it actually feels like to use it.
```

**When to use:** Concepts the user will need to *apply* (not just understand) — practical skills, heuristics, debugging instincts, anything where "what's the rule of thumb" matters more than formal definition.

---

## Choosing a mode

| If the concept has... | Lean toward |
|---|---|
| A clear failure mode people fall into | Detective Mode |
| Hierarchical/prerequisite structure | Adventure/RPG Mode or Escape Room Mode |
| Strict sequencing of steps | Escape Room Mode or Cooking Recipe Mode |
| Composable small parts → bigger whole | Lego Mode |
| A transformation or before/after | Movie Mode or Story Mode |
| A practical skill to apply, not just know | Coaching Mode |
| Emotional/intuitive stakes worth dramatizing | Story Mode |

## Staying in register

Once a mode is chosen, every sentence should sound like it belongs to that mode's "world" until the very last line, where you explicitly tie back to real terminology (per Step 3 of the main SKILL.md). Don't write three sentences of Detective Mode and then suddenly write "basically what's happening technically is..." in a flat explanatory voice — that's the switching-register failure mode. Land the explicit mapping *inside* the mode's voice (e.g., the Director's Commentary aside, the Case Closed line, the Chef's Note) rather than breaking out of it.

# ConceptLens

*Every concept, explained through your world.*

ConceptLens is a Claude skill that re-explains any idea through the user's own mental models — not just one analogy, but a full tournament of framings (domain analogies, narrative modes, and cross-domain reasoning) until something clicks. It then builds an in-session "Analogy Profile" so every later explanation in the conversation is automatically routed through whatever lens already worked for that user.

## What's in this package

```
concept-lens/
├── README.md                       — this file
├── concept-lens.skill               — packaged skill, ready to install
├── SKILL.md                         — the skill definition (also inside the .skill file)
└── references/
    ├── domain-library.md            — 22 domain analogy libraries with worked examples
    └── narrative-modes.md           — 8 narrative-mode templates
```

The `.skill` file is the installable artifact — it already bundles `SKILL.md` and the `references/` folder inside it. The loose `SKILL.md` and `references/` folder are included alongside it too, in case you want to read, edit, or re-package the source directly instead of unzipping the `.skill` file.

## How it works

1. **Read the user's world** — ConceptLens scans the conversation for domain vocabulary, hobbies, professions, or examples the user has already used.
2. **Choose a mode** based on what the user asked for:
   - A clear domain cue ("I'm a chef") → go deep on that one domain.
   - "Give me different ways" → run an **Analogy Tournament**: 5–10 domains, one explanation each, pick-your-favorite.
   - "Explain it as a [story/detective/adventure/etc.]" → activate that **Narrative Mode**.
   - "Explain X using Y" → **Cross-Domain Reasoning**, mapping every component of X onto Y explicitly.
3. **Deliver the translation**, always mapping specific components to specific counterparts (never a vague "it's like a brain but for computers") and always naming where the analogy breaks down.
4. **Update an in-session Analogy Profile** — once a framing clicks, route future explanations through it automatically without announcing it.

## Reference files

- **`references/domain-library.md`** — Gaming, cooking, cricket, football, construction, medicine, finance, music, military, space, gardening, theater, law, restaurant ops, basketball, biology, travel, retail, poker, theme parks, aviation, and more. Each domain includes its core mental model, vocabulary, a fully worked example mapping a real technical concept onto it, and an explicit "where it breaks down" note.
- **`references/narrative-modes.md`** — Story, Detective, Adventure/RPG, Movie, Escape Room, Lego, Cooking Recipe, and Coaching modes, each with a fill-in-the-blank template and guidance on which concept shapes suit it best.

## Installing

Upload or drag the `concept-lens.skill` file into Claude (claude.ai, Claude Code, or Cowork) wherever skill installation is supported. Once installed, the skill triggers automatically whenever you ask for an analogy, say "explain it like I'm a [X]", "ELI5", "I still don't get it", "give me a different way to think about this", or ask for any concept to be explained from scratch.

## Customizing

- To add a domain, follow the existing pattern in `domain-library.md`: core model → vocabulary → one worked example with explicit component mapping → a "breaks down at" line.
- To add a narrative mode, follow the pattern in `narrative-modes.md`: register description → template → "when to use" guidance.
- After editing `SKILL.md` or the reference files, re-package with skill-creator's `package_skill.py` script (or ask Claude to do it for you) to regenerate the `.skill` file.

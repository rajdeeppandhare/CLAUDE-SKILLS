---
name: content-craft
description: Elevates vague or rough content requests — taglines, social captions, video/ad scripts, blog hooks, email subjects, LinkedIn posts, founder-story content, brand voice copy — into premium, emotionally-resonant copy using proven enhancement frameworks instead of generic AI-sounding output. Use this whenever someone gives a thin brief ("promote my product", "write a caption for this", "give me a tagline", "make this sound more premium/viral/Apple-like"), pastes rough draft copy to punch up, or asks for hooks, scripts, taglines, captions, or brand messaging for ANY kind of content — not just marketing/ads. Always trigger before writing generic copy from a one-line request; the missing context is usually the real problem, not the model.
---

# Content Craft

Most "make this better" content requests fail for one reason: the brief is too thin. A request like "promote our coffee shop" forces a generic answer because there's no brand personality, audience, emotion, or outcome to write toward. The fix isn't a smarter model — it's filling in that context before generating, then routing to the right structural framework for the format being asked for.

This skill turns thin requests into specific, premium-feeling content by doing two things in order: **(1) fill the context gap, (2) apply the right framework for the format.**

## Step 1: Fill the context gap (BAEO)

Before writing anything, make sure you have — or can reasonably infer — these four things. If 2+ are missing and the request is non-trivial (not a quick one-liner), ask the user briefly rather than guessing blind. If the request is casual/low-stakes, infer sensible defaults and state the assumption in one line, then proceed.

- **B — Brand/Subject**: What is this, actually? What's its personality in 2-3 adjectives (warm, ambitious, minimalist / bold, irreverent, fast / luxury, quiet, exacting)?
- **A — Audience**: Who exactly is this for? Age range, role, mindset, or situation — not "everyone."
- **E — Emotion**: What feeling should this land on? (belonging, relief, ambition, FOMO, trust, pride, calm)
- **O — Outcome**: What should the reader/viewer do or feel immediately after? (click, share, remember, buy, feel seen)

A strong brief sounds like: *"You're a senior brand strategist. Create [output] for a [brand + personality] targeting [audience]. Focus on [emotion] and [outcome]."* — that's the BAEO formula in sentence form. If the user already gave this much detail, skip straight to Step 2.

If the user pastes existing rough copy instead of describing from scratch, reverse-engineer BAEO from what's there (or what's clearly missing) before rewriting it.

## Step 2: Pick the framework for the format

Different content formats need different shapes — a tagline and a 30-second video script are not the same writing problem. Match the request to a framework below, then read the matching section in `references/frameworks.md` for the full pattern, structure, and worked examples before writing.

| User asks for... | Framework | Reference section |
|---|---|---|
| Tagline, slogan, one-liner | Brand + Audience + Emotion + Outcome compression | `frameworks.md#tagline` |
| Social caption, post copy | Hook → Emotion → Benefit → CTA | `frameworks.md#caption` |
| Ad script, video script, reel/short script | Problem → Ritual/Shift → Payoff → Brand line | `frameworks.md#video-script` |
| Blog/article opening, landing page hero | Open-loop, contrarian-claim, or specific-stat hook | `frameworks.md#blog-hook` |
| Email subject line | Curiosity-gap or specificity-over-cleverness | `frameworks.md#email-subject` |
| LinkedIn / thought-leadership post | Hook → Insight → Story → Lesson → CTA | `frameworks.md#linkedin` |
| Founder story, About page, brand origin content | Before → Struggle → Insight → Transformation → Invitation | `frameworks.md#founder-story` |
| "Make it sound like Apple/Nike," minimalist premium tone | Declarative-sentence, verb-first principles | `frameworks.md#minimalist-voice` |

If the request doesn't cleanly match a row, default to the Caption framework (Hook → Emotion → Benefit → CTA) — it generalizes well to almost any short-form copy.

For tone calibration beyond structure (word choice, sentence rhythm, what "luxury" vs "playful" vs "bold" actually sounds like on the page), check `references/voice-styles.md`.

## Step 3: Generate with a quality bar

- **Avoid these by default** — they're the generic-AI tells: "Take your business to the next level," "Unlock your potential," "Fresh [product] every day," "In today's fast-paced world," "Experience the difference," any sentence that could be copy-pasted onto a competitor's brand with zero edits.
- **Specificity beats cleverness.** A concrete detail ("the first promise you make before 9am") lands harder than an abstract claim ("we care about quality").
- **Write toward the one emotion from Step 1**, not three at once. Diffuse copy reads as generic copy.
- **Match sentence rhythm to brand personality.** Minimalist/premium = short, declarative, lots of white space. Warm/community = longer, second-person, conversational. Bold/disruptive = fragments, contrast, punch.

## Step 4: Deliver options, not one answer

Unless the user asked for exactly one version, give **3-5 variants** that vary in register (e.g. one safe/clear, one bold/punchy, one premium/minimal) rather than 5 near-duplicates of the same idea. Label each variant by its angle in 2-4 words so the user can pick by instinct, not re-read everything. For longer formats (scripts, founder stories), one strong version plus a one-line note on what alternate angle was considered is usually better than 5 full scripts.

If the user shares a rough draft to "punch up" rather than asking from scratch, show the before → after so the improvement is visible, and briefly name which framework/principle did the work (this teaches them to write better briefs next time, per the model-vs-prompt insight this skill is built on).

See `references/examples.md` for full worked before/after examples across multiple content types (not just marketing) — coffee brand, SaaS product, and fitness app — to calibrate output quality before responding.

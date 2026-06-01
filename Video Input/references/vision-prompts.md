# 🔍 Claude Vision Prompt Library

Reference guide for all video analysis prompts. Pass these to `analyse_frames.py`
or use inline in Vision API calls. Organised by task category.

---

## Built-in Shorthands

These are available directly in `analyse_frames.py` as shorthand keys:

| Key | Task |
|---|---|
| `summarise` | General chronological summary |
| `objects` | Object / entity inventory |
| `scenes` | Scene change detection |
| `events` | Action and event timeline |

---

## 🎬 Summary & Narrative

### General Summary
```
These frames are sampled evenly from a video. Describe what happens
chronologically — cover the main content, any people, settings,
and key events visible. Be concise but thorough.
```

### Detailed Narrative
```
These frames are sampled evenly from a video. Tell me:
1. What is the overall topic or story?
2. What happens in the opening, middle, and final sections?
3. Are there any notable people, brands, or on-screen text?
4. What is the mood or tone throughout?
```

### Executive Summary (1 paragraph)
```
Summarise this video in a single paragraph of no more than 5 sentences.
Focus on: what it is, who it's for, and what the key takeaway is.
```

---

## 👁️ Object & Entity Detection

### Full Object Inventory
```
List every distinct object, person, animal, or entity visible across
these video frames. For each item note: what it is, how often it appears
(rare / occasional / frequent), and which frames it's most visible in.
```

### People & Demographics
```
Describe all people visible in these video frames:
- Approximate total count
- Gender presentation and approximate age range for each
- Clothing and distinguishing features
- What each person is doing
```

### Product & Brand Detection
```
Identify any products, brands, logos, or commercial items visible
in these video frames. For each: brand name, product type,
and how prominently it features (background / incidental / focal point).
```

### Text & Graphics Extraction
```
Extract all visible text from these video frames: captions, lower-thirds,
titles, subtitles, logos, watermarks, on-screen graphics, and UI labels.
List them in the order they appear.
```

---

## 🎭 Scene & Environment Analysis

### Scene Change Detection
```
Identify every distinct scene across these video frames. For each scene:
- What is the setting / environment?
- What is happening?
- Approximately when does it appear (early / mid / late in the video)?
- What triggers the scene change?
```

### Environment Classification
```
Classify the environment shown in each frame:
indoor / outdoor, urban / rural / studio / digital / other.
Note lighting conditions (natural / artificial / mixed) and time of day if visible.
```

### Camera & Production Analysis
```
Analyse the production style of this video:
- Camera angles and movement (static, handheld, drone, etc.)
- Editing style (fast cuts, slow transitions, etc.)
- Production quality (amateur, professional, broadcast)
- Any special effects, graphics overlays, or animations
```

---

## ⚡ Action & Event Detection

### Event Timeline
```
What actions or events occur in this video? For each event:
- Describe what happens
- Note when it occurs (early / mid / late)
- Rate its significance (minor / notable / key moment)
Present as a timeline list.
```

### Sports & Activity Recognition
```
Identify the sport or physical activity shown. Describe:
- The sport / activity name
- Key actions and techniques visible
- Athletes or participants present
- Any visible scores, results, or game state
```

### Tutorial / Instructional Analysis
```
This appears to be a tutorial or instructional video. Identify:
- The skill or task being taught
- Step-by-step actions demonstrated (numbered list)
- Tools, materials, or software used
- Any key tips or techniques highlighted
```

---

## 🛡️ Safety & Moderation

### Content Safety Review
```
Review these video frames and flag any content that may be:
- Violent, graphic, or disturbing
- Sexually explicit or suggestive
- Hateful, discriminatory, or extremist
- Depictions of illegal or dangerous activity
- Unsuitable for general audiences

Rate overall: ✅ Safe / ⚠️ Review needed / 🚫 Unsafe
Explain your reasoning for any flags.
```

### Age Appropriateness
```
Assess the age appropriateness of this video content:
- All ages / 13+ / 16+ / 18+
List any specific content that informed your rating.
```

---

## 🔬 Specialist Use Cases

### Accessibility Description
```
Write an audio description of this video suitable for visually impaired viewers.
Describe the visual content in clear, plain language — what's on screen,
who's present, what's happening, and any text visible.
```

### Medical / Scientific Observation
```
Describe the scientific or medical content visible in these frames objectively.
Note any procedures, equipment, specimens, or data visualisations shown.
Do not provide diagnosis or medical advice.
```

### Real Estate / Space Analysis
```
Analyse the space or property shown in these video frames:
- Room types and layout
- Approximate size and condition
- Notable features, finishes, or fixtures
- Natural light and outlook
```

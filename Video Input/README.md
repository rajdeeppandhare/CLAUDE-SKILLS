# 🎬 Video Input — Claude Skill

> A multimedia skill for Claude that reads, transcribes, and analyses video files and YouTube links — turning raw footage into insights with no setup required.

---

## ✨ What It Does

Drop in any video file or URL and Video Input will:

- 🖼️ **Extract frames** — pulls evenly-spaced screenshots throughout the video
- 🎙️ **Transcribe speech** — converts audio to accurate, timestamped text via Whisper
- 🔍 **Analyse content** — uses Claude Vision to describe, summarise, and interpret what's on screen
- 🎯 **Detect objects, scenes & events** — identifies people, items, scene changes, and actions
- 🌐 **Download from URLs** — works with YouTube, Vimeo, and direct video links

---

## 🎥 What It Can Analyse

| Category | Examples |
|---|---|
| 🟣 Transcription | Lectures, interviews, podcasts, meetings, tutorials |
| 🔵 Scene Detection | Scene changes, setting descriptions, environment analysis |
| 🟢 Object & People | Person count, objects present, brand/product spotting |
| 🟡 Event Detection | Actions, activities, key moments, sports plays |
| 🔴 Content Moderation | Flagging graphic, sensitive, or unsafe content |

**Supported formats**: mp4, mov, avi, mkv, webm, flv, wmv — and any YouTube or direct video URL.

---

## 📦 Installation

### Option 1 — Install the `.skill` file
1. Download `video-input.skill`
2. In Claude.ai, go to **Settings → Skills**
3. Click **Install Skill** and upload the file

### Option 2 — Install from folder
1. Clone or download this repo
2. Zip the `video-input/` folder
3. Rename to `video-input.skill`
4. Install via Claude.ai **Settings → Skills**

---

## 🚀 How to Use

Once installed, just talk to Claude naturally:

```
"Analyse this video"
"Transcribe this clip"
"What's happening in this video?"
"Extract frames from this video"
"Summarise this YouTube video: https://..."
"What objects are in this video?"
"Detect the scene changes"
```

Then attach your video file or paste a URL — Claude runs the full pipeline automatically.

---

## 📊 Example Output

```
## 🎬 Video Analysis Report
Source: product-demo.mp4  
Duration: 3m 42s  
Frames analysed: 20  
Audio: Transcribed (Whisper base)

---

### 📝 Transcript (excerpt)
[0.0s → 4.2s] Welcome to the product demo. Today I'll be showing you...
[4.2s → 9.8s] First, let's take a look at the dashboard...

---

### 🔍 Visual Analysis

**Summary**: The video is a software product demo recorded from a screen share.
The presenter walks through a web dashboard, highlighting analytics charts,
a user management panel, and an export feature. A second person joins via
video call at the 2-minute mark.

**Scenes detected**:
- 0:00–1:15 — Desktop screen share, dashboard overview
- 1:15–2:00 — Drill-down into analytics charts
- 2:00–3:42 — Video call with second participant, live Q&A

**Objects & people**:
- 1 presenter (off-screen voice), 1 remote participant (video call)
- Web browser, data charts, sidebar navigation, export modal

---

### ✅ Key Moments
- 0:32 — Dashboard first shown
- 1:18 — Chart interaction demonstrated  
- 2:04 — Second participant joins
- 3:10 — Export feature walkthrough
```

---

## 📁 Skill Structure

```
video-input/
├── SKILL.md                    # Core skill logic & pipeline instructions
├── README.md                   # This file
├── video-input.skill           # Installable skill package
└── references/
    ├── vision-prompts.md       # Full library of Claude Vision prompts by task
    └── format-compatibility.md # Supported formats, limits, and tuning guide
```

---

## 🛠️ Under the Hood

| Component | Purpose |
|---|---|
| **ffmpeg** | Frame extraction, audio separation, format conversion |
| **OpenCV** | Precise frame sampling and JPEG export |
| **OpenAI Whisper** | Local speech-to-text (base model by default) |
| **yt-dlp** | YouTube and web video downloading |
| **Claude Vision** | Visual understanding and content analysis |

Everything runs locally in Claude's environment — no external APIs beyond Anthropic's own.

---

## 🧠 Built With

- [Claude Skills](https://claude.ai) — Anthropic's skill system for extending Claude
- [OpenAI Whisper](https://github.com/openai/whisper) — Open-source speech recognition
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) — Video downloading library
- [FFmpeg](https://ffmpeg.org/) — Industry-standard multimedia processing

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built as a portfolio project demonstrating Claude skill development and multimedia AI pipelines.*

---
name: video-input
description: >
  Use this skill whenever the user provides a video file or video URL (YouTube, Vimeo, direct links)
  and wants to do anything with it — extracting frames, transcribing speech, summarising or
  analysing content, or detecting objects/scenes/events. Trigger on phrases like "analyse this
  video", "transcribe the video", "what's in this video", "extract frames", "summarise the video",
  "detect scenes", or any upload of a .mp4, .mov, .avi, .mkv, or similar file. Always use this
  skill rather than improvising — it encodes the exact ffmpeg/Whisper/Claude-Vision pipeline that
  reliably works in this environment.
---

# Video Input Skill

Handles **all** video-related tasks end-to-end: local files, YouTube/web URLs, and frame sequences.

| Goal | Pipeline |
|---|---|
| Extract frames | ffmpeg + OpenCV → JPEG frames |
| Transcribe audio | ffmpeg → Whisper STT |
| Summarise / analyse | frames → Claude Vision |
| Detect objects / scenes / events | frames → Claude Vision (targeted prompt) |

---

## Step 0 — Resolve the video source

**Uploaded file** (path under `/mnt/user-data/uploads/`):
→ Use path directly. Copy to `/tmp/video_input.<ext>` if needed.

**YouTube / web URL**:
→ Run `scripts/download_video.py <url>` → outputs `/tmp/video_input.mp4`

**Frame sequence / images already extracted**:
→ Skip to Step 2 — analyse frames directly.

---

## Step 1 — Extract what you need

| Script | What it does |
|---|---|
| `scripts/extract_frames.py` | N evenly-spaced JPEG frames from the video |
| `scripts/extract_audio.py` | Audio track → `/tmp/video_audio.mp3` |
| `scripts/transcribe.py` | Full speech-to-text via Whisper (base model) |
| `scripts/download_video.py` | Download from YouTube/URL via yt-dlp |

All scripts print their output path as the last line for easy parsing.

**Dependencies** (auto-available in this environment):
- `ffmpeg` — video/audio processing
- `opencv-python` — frame extraction
- `openai-whisper` — speech transcription
- `yt-dlp` — video downloading

---

## Step 2 — Analyse frames with Claude Vision

After extracting frames, base64-encode them and call the Anthropic API:

```python
import base64, anthropic, glob

client = anthropic.Anthropic()
frames = sorted(glob.glob("/tmp/video_frames/*.jpg"))[:20]  # cap at 20

content = []
for f in frames:
    with open(f, "rb") as fh:
        b64 = base64.b64encode(fh.read()).decode()
    content.append({"type": "text", "text": f"[{os.path.basename(f)}]"})
    content.append({"type": "image", "source": {
        "type": "base64", "media_type": "image/jpeg", "data": b64}})

content.append({"type": "text", "text": YOUR_PROMPT})

response = client.messages.create(
    model="claude-opus-4-5-20251101",
    max_tokens=2000,
    messages=[{"role": "user", "content": content}]
)
print(response.content[0].text)
```

**Prompt shorthands** (see `references/vision-prompts.md` for full library):

| Shorthand | Use case |
|---|---|
| `summarise` | Chronological narrative summary |
| `objects` | Every entity / object visible |
| `scenes` | Scene changes + descriptions |
| `events` | Actions, events, and when they occur |

Or use `scripts/analyse_frames.py <frames_dir> "<prompt_or_shorthand>"` directly.

---

## Step 3 — Combine transcript + vision (for full analysis)

For the richest output, run both pipelines and merge context:

```python
# Pass transcript as additional context in the Vision prompt
final_prompt = VISION_PROMPT + f"\n\nAudio transcript for context:\n{transcript}"
```

---

## Step 4 — Present results

| Output type | Presentation |
|---|---|
| Transcription | Formatted text, offer to save as `.txt` |
| Frame analysis | Narrative prose, structured list, or table |
| Extracted frames | `present_files` to show frame grid |
| Full analysis | Transcript + scene analysis side-by-side |

---

## Quick-start recipes

### "Summarise this video"
1. `extract_frames.py` (20 frames)
2. `transcribe.py` for audio context
3. Vision: `summarise` prompt + transcript as context

### "Transcribe this video"
1. `extract_audio.py`
2. `transcribe.py` → return formatted text

### "What objects/people are in this video?"
1. `extract_frames.py` (10–20 frames)
2. Vision: `objects` prompt

### "Detect scene changes"
1. `extract_frames.py` (30+ frames for accuracy)
2. Vision: `scenes` prompt

---

## Limits & tuning

- **Whisper model size**: default is `base` (fast). Change `MODEL_SIZE` in `transcribe.py` to `small` or `medium` for better accuracy.
- **Frame count**: 10–20 covers most videos. Use 30+ for fine-grained scene detection.
- **Vision context cap**: ~20 images per API call. Batch frames for long videos.
- **Supported formats**: mp4, mov, avi, mkv, webm, flv, wmv — anything ffmpeg handles.
- **YouTube**: yt-dlp selects best mp4 ≤ 720p by default.

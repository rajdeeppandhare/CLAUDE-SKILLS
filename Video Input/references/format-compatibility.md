# 📋 Format Compatibility & Tuning Guide

Reference for supported video formats, known limitations, and tuning options
for each component of the Video Input skill pipeline.

---

## 🎬 Supported Input Formats

### Video Files (via ffmpeg)

| Format | Extension | Notes |
|---|---|---|
| MP4 | `.mp4` | Best supported — recommended default |
| QuickTime | `.mov` | Full support |
| AVI | `.avi` | Full support |
| Matroska | `.mkv` | Full support |
| WebM | `.webm` | Full support |
| Flash Video | `.flv` | Full support |
| Windows Media | `.wmv` | Full support |
| MPEG | `.mpeg`, `.mpg` | Full support |
| 3GPP | `.3gp` | Full support (mobile video) |
| MXF | `.mxf` | Partial — depends on codec |

> **Rule of thumb**: if ffmpeg can play it, this skill can process it.
> Run `ffmpeg -i <file>` to check codec compatibility.

### Web Sources (via yt-dlp)

| Source | Support level |
|---|---|
| YouTube | ✅ Full — all public videos |
| Vimeo | ✅ Full — public videos |
| Twitter/X | ✅ Full |
| Instagram | ⚠️ Public posts only |
| TikTok | ⚠️ Public posts only |
| Direct MP4 URLs | ✅ Full |
| Twitch VODs | ✅ Full |
| Facebook | ⚠️ Public posts only |
| Age-restricted / private | ❌ Not supported |

---

## 🔧 Frame Extraction Tuning

**Script**: `scripts/extract_frames.py`

| Parameter | Default | Recommendation |
|---|---|---|
| `num_frames` | 20 | Increase to 30–50 for scene detection |
| `output_dir` | `/tmp/video_frames` | Change if running multiple videos |
| JPEG quality | 85 | Lower to 70 for faster Vision calls |

### Frame count guidelines

| Video length | Recommended frames | Use case |
|---|---|---|
| < 1 min | 10 | Quick summary |
| 1–5 min | 20 | Standard analysis |
| 5–15 min | 30 | Scene detection |
| 15–60 min | 40–50 | Long-form content |
| 60+ min | 50 (batched) | Use Vision batching |

### Vision context limit
Claude Vision accepts **up to ~20 images per API call**. For longer videos:
1. Extract 40+ frames
2. Send in two batches of 20
3. Merge the two analyses in a follow-up prompt

---

## 🎙️ Whisper Transcription Tuning

**Script**: `scripts/transcribe.py`

### Model size comparison

| Model | Speed | Accuracy | VRAM | Best for |
|---|---|---|---|---|
| `tiny` | Fastest | Low | ~1 GB | Quick previews |
| `base` | Fast | Good | ~1 GB | **Default — balanced** |
| `small` | Moderate | Better | ~2 GB | Interviews, lectures |
| `medium` | Slow | High | ~5 GB | Technical content |
| `large` | Slowest | Best | ~10 GB | Critical transcription |

Change by editing `MODEL_SIZE = "base"` in `transcribe.py`.

### Language support
Whisper auto-detects language. For non-English content, force the language:
```python
result = model.transcribe(audio_path, language="fr")  # French
result = model.transcribe(audio_path, language="es")  # Spanish
result = model.transcribe(audio_path, language="de")  # German
```
Supports 99 languages. See [Whisper docs](https://github.com/openai/whisper) for full list.

### Audio quality tips
- Whisper performs best on clean speech (minimal background noise)
- For noisy audio, pre-process with ffmpeg: `ffmpeg -i input.mp4 -af "anlmdn" output.mp3`
- Music-heavy content will produce lower accuracy transcription

---

## 📥 Download Tuning (yt-dlp)

**Script**: `scripts/download_video.py`

### Default format selection
```
bestvideo[ext=mp4][height<=720]+bestaudio[ext=m4a]/best[ext=mp4]/best
```
Prefers: MP4 container, max 720p, best available audio.

### Common overrides

| Need | Format string |
|---|---|
| High quality (1080p) | `bestvideo[height<=1080]+bestaudio/best` |
| Audio only | `bestaudio/best` |
| Small file (360p) | `bestvideo[height<=360]+bestaudio/best` |
| Fastest download | `worstvideo+worstaudio/worst` |

### Rate limits
yt-dlp respects YouTube's rate limits automatically. For large batches, add:
```python
ydl_opts["sleep_interval"] = 2  # 2 second delay between downloads
```

---

## ⚠️ Known Limitations

| Limitation | Detail | Workaround |
|---|---|---|
| Vision cap | ~20 images per API call | Batch in groups of 20 |
| Whisper speed | Large model is slow on CPU | Use `base` or `small` |
| Private videos | yt-dlp can't access private/auth-gated content | Download manually first |
| Live streams | Not supported | Record first, then analyse |
| Very long audio | Whisper may time out on 2hr+ files | Split audio with ffmpeg |
| Non-speech audio | Music, SFX produce poor transcripts | Skip transcription step |

### Splitting long audio files
```bash
ffmpeg -i long_audio.mp3 -f segment -segment_time 600 -c copy part_%03d.mp3
# Creates 10-minute chunks: part_000.mp3, part_001.mp3, ...
```
Then transcribe each chunk and concatenate the results.

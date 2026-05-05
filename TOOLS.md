# 🔧 Tools & Technologies — PawCoach

A full breakdown of every tool, library, and service used in PawCoach, with the role each plays in the system.

---

## Frontend & UI

| Tool | Version | Role |
|------|---------|------|
| [Streamlit](https://streamlit.io) | Latest | Main web application framework — renders all tabs, sidebar, buttons, audio recorder, and result displays |
| [audio-recorder-streamlit](https://github.com/Joooohan/audio-recorder-streamlit) | Latest | In-browser microphone recording widget used in Tab 2 (voice) and Tab 3 (bark) |
| [streamlit-extras](https://extras.streamlit.app) | Latest | Additional Streamlit utility components |
| Google Fonts — Syne + DM Sans | CDN | Custom typography for the dark-themed UI |
| Custom CSS | — | Dark mode theme (`#0e1117` background), score cards, panel labels, button styling |

---

## Image Generation

| Tool | Role |
|------|------|
| [Pollinations.ai](https://pollinations.ai) | Free, no-signup image generation API. Receives a text prompt via HTTP GET and returns a 512×512 PNG. No API key required. |
| **Flux model** (via Pollinations) | The underlying generative model used for photorealistic dog training images |
| [Pillow (PIL)](https://python-pillow.org) | Image loading, compositing, and grid stitching for the 4-panel output |

**Prompt system:** 4 panel templates with 5 injected variables (`dog_breed`, `dog_color`, `trainer`, `environment`, `command`). A structured negative prompt suppresses common failure artifacts (blur, deformed anatomy, cartoon style).

---

## Voice Analysis

| Tool | Role |
|------|------|
| [librosa](https://librosa.org) | Core audio DSP library. Used for duration measurement (`get_duration`), volume/RMS (`feature.rms`), and pitch tracking (`piptrack`) |
| [OpenAI Whisper](https://github.com/openai/whisper) (`base` model) | Speech-to-text transcription for the clarity dimension — verifies the correct command word was spoken |
| [soundfile](https://pysoundfile.readthedocs.io) | WAV file writing for Whisper input |
| [ffmpeg](https://ffmpeg.org) | System-level audio format conversion. Handles browser-native webm/ogg → WAV conversion (critical — librosa silently hangs on non-WAV formats) |
| [NumPy](https://numpy.org) | Numerical operations on audio arrays (mean, log, array slicing) |
| [Matplotlib](https://matplotlib.org) | Waveform and pitch contour visualization rendered as PNG and displayed in Streamlit |

**Science standards source:** Thresholds for duration, volume, and pitch contour are derived from:
- Pryor, K. (2009). *Reaching the Animal Mind*
- Hiby, E.F. et al. (2004). *Animal Welfare*, 13, 63–69

---

## Bark Emotion Recognition

| Tool | Role |
|------|------|
| [librosa](https://librosa.org) | Spectral centroid extraction (`feature.spectral_centroid`) and bark onset detection (`onset.onset_detect`) |
| Rule-based fusion system | Custom lookup table: acoustic cluster × situational context → emotion label + training advice. No ML training required. |
| [Matplotlib](https://matplotlib.org) | Mel spectrogram and onset strength visualization |

**Acoustic clusters classified:** `high_freq_fast` · `high_freq_slow` · `low_freq_fast` · `low_freq_slow`  
**Emotion states recognized:** 8 states including Excited, Alert, Anxious, Distressed, Play-soliciting, Calm, Seeking Attention, Happy

---

## Deployment & Infrastructure

| Tool | Role |
|------|------|
| [Google Colab](https://colab.research.google.com) | Cloud notebook environment providing free T4 GPU compute |
| [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) (`cloudflared`) | Exposes the local Streamlit server (port 8501) as a public HTTPS URL. Free, no account required. |
| [Python subprocess](https://docs.python.org/3/library/subprocess.html) | Process management — starts Streamlit server in background, runs cloudflared tunnel |

---

## Python Standard Library

| Module | Usage |
|--------|-------|
| `io` | In-memory byte streams for image and audio buffers |
| `os` | Temp file cleanup |
| `tempfile` | Secure temporary file creation for audio processing |
| `time` | Request throttling between image generation calls |
| `re` | Regex extraction of public URL from cloudflared output |
| `urllib.parse` | URL encoding of image generation prompts |

---

## Full Package Install List

```bash
pip install diffusers transformers accelerate torch Pillow
pip install streamlit
pip install librosa soundfile
pip install openai-whisper
pip install audio-recorder-streamlit
pip install streamlit-extras
pip install requests
apt-get install -y ffmpeg
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64
```

---

## Architecture Summary

```
Browser (User)
    │
    ▼
Streamlit UI (app.py) ── Sidebar: Dog Profile
    │
    ├── Tab 1: Image Generation
    │       └── HTTP GET → Pollinations.ai (Flux) → PIL → Display
    │
    ├── Tab 2: Voice Analysis
    │       └── audio-recorder → ffmpeg → librosa + Whisper → Score + Plot
    │
    └── Tab 3: Bark Emotion
            └── audio-recorder → librosa → Acoustic Cluster × Context → Emotion
    │
    ▼
Cloudflare Tunnel → Public HTTPS URL
    │
    ▼
Google Colab T4 GPU (compute backend)
```

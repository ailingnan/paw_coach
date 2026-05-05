# 🐾 PawCoach — AI-Powered Dog Training Assistant

> *"Dogs don't respond to words — they respond to the energy, tone, and consistency behind them. PawCoach makes the invisible visible."*

PawCoach is a multimodal AI platform that democratizes professional-quality dog training. It addresses a clear market gap: while nearly **67% of dog owners** attempt self-training, approximately **40% give up within three months** — primarily due to lack of expert feedback and inability to understand their dog's emotional state.

---

## ✨ Features

### 📸 Tab 1 — Visual Training Guide
Generates a personalized **4-panel photo-realistic training comic** tailored to your dog's breed, coat color, training environment, and the specific command being taught. Each panel represents a distinct step in the training sequence:

| Panel | Step | Description |
|-------|------|-------------|
| 1 | Setup | Owner and dog prepare together |
| 2 | Signal | Owner delivers hand signal + verbal command |
| 3 | Execute | Dog performs the command correctly |
| 4 | Reward | Positive reinforcement with treat |

Images are generated via **Pollinations.ai (Flux model)** — free, no API key required.

---

### 🎙️ Tab 2 — Voice Command Analyzer
Record yourself giving a command. PawCoach evaluates your voice across **4 science-backed dimensions**:

| Dimension | What It Measures | Why It Matters |
|-----------|-----------------|----------------|
| Duration | How long the command takes | Short commands are clearer for dogs |
| Volume | RMS energy in dB | Assertive tone gets attention |
| Pitch Contour | Does tone drop at end? | Rising pitch sounds like a question |
| Clarity | Whisper transcription match | Enunciation directly affects learning |

Thresholds are sourced from animal behavior research (Pryor 2009, Hiby et al. 2004). Results include a **0–100 score**, per-dimension pass/fail, waveform, and pitch contour visualization.

---

### 🐶 Tab 3 — Bark Emotion Recognizer
A **multimodal fusion** system combining acoustic analysis + situational context to classify your dog's emotional state into one of **8 categories**, each with a plain-English training recommendation.

**Acoustic features extracted:**
- Spectral centroid (perceived pitch proxy)
- Onset rate (bark burst frequency)

**5 situational contexts supported:** Excitement · Alert · Distress · Play · Calm

---

## 🗂️ Repository Structure

```
pawcoach/
├── app.py                  # Main Streamlit application
├── PawCoach_Colab_Cells.txt  # Colab notebook cells (copy-paste format)
├── README.md               # This file
├── SETUP.md                # Step-by-step setup instructions
├── TOOLS.md                # Tools and libraries used
└── sample_outputs/
    ├── voice_analysis_sample.png
    ├── bark_emotion_sample.png
    └── training_guide_sample.png
```

---

## 🚀 Quick Start

See **[SETUP.md](SETUP.md)** for full step-by-step instructions.

**TL;DR:**
1. Open Google Colab with a T4 GPU runtime
2. Run cells 1–9 from `PawCoach_Colab_Cells.txt` in order
3. Click the `trycloudflare.com` URL printed by Cell 9

---

## 📊 Experiment Results

### Image Quality — Structured vs. Baseline Prompts (40 images, 5 runs)

| Metric | Structured Prompts | Baseline Prompt | Improvement |
|--------|-------------------|-----------------|-------------|
| Dog Color Correct | 0.92 | 0.44 | +0.48 |
| Breed Accuracy | 0.80 | 0.32 | +0.48 |
| Scene Correct | 0.96 | 0.60 | +0.36 |
| Prop Present | 0.72 | 0.20 | +0.52 |
| Action Correct | 0.84 | 0.36 | +0.48 |
| **Single-Panel Score** | **4.24/5** | **1.92/5** | **+2.32** |
| Cross-Panel Consistency | 3.8/5 | 1.6/5 | +2.2 |

### Voice Analysis — Sample Session (Command: "Sit")

| Dimension | Measured | Target | Result |
|-----------|----------|--------|--------|
| Duration | 0.82s | < 1.2s | ✅ Pass |
| Volume | 67.3 dB | > 60 dB | ✅ Pass |
| Pitch Contour | Rising | Must Drop | ❌ Fail |
| Clarity | "sit" | sit | ✅ Pass |
| **Overall Score** | **75/100** | — | Needs Work |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Streamlit, custom CSS, Google Fonts (Syne + DM Sans) |
| Image Generation | Pollinations.ai (Flux model) |
| Voice Analysis | librosa, OpenAI Whisper Base, soundfile, ffmpeg |
| Bark Analysis | librosa DSP, multimodal rule-based fusion |
| Deployment | Google Colab T4 GPU + Cloudflare Tunnel |

See **[TOOLS.md](TOOLS.md)** for detailed tool descriptions.

---

## 📚 References

- Pryor, K. (2009). *Reaching the Animal Mind*. Scribner.
- Hiby, E.F., Rooney, N.J., & Bradshaw, J.W.S. (2004). Dog training methods: Their use, effectiveness and interaction with behaviour and welfare. *Animal Welfare*, 13, 63–69.

---

*Built as a Week 14 Final Project · Google Colab · Streamlit · Pollinations.ai · Whisper · librosa*

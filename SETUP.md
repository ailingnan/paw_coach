# 🛠️ Setup Instructions — PawCoach

Complete step-by-step guide to running PawCoach on Google Colab.

---

## Prerequisites

| Requirement | Details |
|-------------|---------|
| Google Account | Required for Google Colab |
| Runtime | T4 GPU (free tier works) |
| Browser | Chrome or Firefox recommended |
| Time | ~5 minutes setup, first image ~15 seconds |

> **No API keys required.** PawCoach uses Pollinations.ai for image generation (free, no signup) and Cloudflare Tunnel for public URL (free, no signup).

---

## Step 1 — Open Google Colab with GPU

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Create a new notebook
3. Set runtime to GPU:
   - Click **Runtime** → **Change runtime type**
   - Set **Hardware accelerator** to **T4 GPU**
   - Click **Save**

---

## Step 2 — Copy the Cells

Open `PawCoach_Colab_Cells.txt` from this repository.  
Create **10 separate Colab cells** and copy each block in order.

| Cell | Title | Action |
|------|-------|--------|
| Cell 1 | Install Dependencies | Run once per session |
| Cell 2 | app.py Part 1 — Imports & Config | Writes app.py |
| Cell 3 | app.py Part 2 — Constants & Standards | Appends to app.py |
| Cell 4 | app.py Part 3 — Image Generation | Appends to app.py |
| Cell 5 | app.py Part 4 — Voice Analysis | Appends to app.py |
| Cell 6 | app.py Part 5 — Bark Emotion | Appends to app.py |
| Cell 7 | app.py Part 6 — Sidebar | Appends to app.py |
| Cell 8 | app.py Part 7 — Main UI Tabs | Appends to app.py |
| Cell 9 | Launch with Cloudflare Tunnel | Starts the app |
| Cell 10 | (Optional) Stop the App | Shuts everything down |

> ⚠️ **Cells 2–8 must be run in order every time you restart Colab.** They build `app.py` incrementally using `%%writefile`.

---

## Step 3 — Run Cells 1 Through 8

Run each cell **in order** (Shift+Enter or click ▶).

- **Cell 1** installs all Python packages and system dependencies (~2–3 minutes on first run).
- **Cells 2–8** write `app.py` to disk. Each should complete in under 1 second.
- You will see no output for Cells 2–8 if successful — that is normal.

---

## Step 4 — Launch the App (Cell 9)

Cell 9 starts the Streamlit server and opens a Cloudflare Tunnel.

```
⏳ 正在建立隧道，请稍等...
============================================================
🐾 PawCoach is LIVE!
   👉 打开这个链接: https://xxxx-xxxx.trycloudflare.com
============================================================
```

Copy the `trycloudflare.com` URL and open it in your browser.

> Keep Cell 9 **running** the entire time you use the app. Stopping it shuts down the server.

---

## Step 5 — Using PawCoach

### Tab 1 · Visual Training Guide
1. Fill in the **Dog Profile** in the left sidebar (breed, color, environment, command)
2. Click **🎨 Generate Guide**
3. Wait ~15–20 seconds per panel (4 panels total, ~60–80 seconds)
4. Download the full grid with **⬇️ Download Full Grid**

### Tab 2 · Voice Command Analyzer
1. Select your target command in the sidebar
2. Click the **🔴 microphone button** and say the command clearly
3. Click the button again to stop recording
4. Results appear automatically with score and visualizations

### Tab 3 · Bark Emotion Recognizer
1. Select the context (what was happening when your dog barked)
2. Click the **🔵 microphone button** and play or record your dog's bark
3. View the emotion classification and training advice

---

## Stopping the App

Run **Cell 10** to cleanly shut down Streamlit and the tunnel:

```python
from pyngrok import ngrok
import subprocess

ngrok.kill()
subprocess.run(["pkill", "-f", "streamlit"], capture_output=True)
print("✅ PawCoach shut down.")
```

Or simply stop Cell 9 (click the ■ stop button).

---

## Troubleshooting

| Problem | Likely Cause | Fix |
|---------|-------------|-----|
| Cell 9 prints no URL | Tunnel takes longer to start | Wait 10s, re-run Cell 9 |
| Images show "Network Error" | Pollinations.ai rate limit | Wait 30s, click Generate again |
| Audio recording doesn't work | Browser microphone permission | Allow microphone in browser popup |
| Whisper takes very long | First-time model download (~150MB) | Wait, subsequent runs are faster |
| Colab disconnects | Free tier 90-min idle timeout | Re-run cells 1–9 from the top |

---

## Notes

- The Whisper model (~150 MB) downloads automatically on first use of Tab 2.
- All data stays within your Colab session — nothing is stored or sent externally except image generation requests to Pollinations.ai.
- If Colab runtime resets, re-run all cells from Cell 1.

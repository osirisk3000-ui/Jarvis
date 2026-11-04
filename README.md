# J.A.R.V.I.S. — Local AI Assistant

**Just A Rather Very Intelligent System**

A local-first personal assistant with a custom Retro Glass web interface, Ollama chat streaming, browser speech controls, local Piper text-to-speech, ComfyUI image generation, optional ComfyUI video workflows, web search, file uploads, and a small browser-based code workspace.

> This repository contains application source code, not the large AI models. Ollama models, ComfyUI checkpoints/video models, and Piper voice models are installed separately.

## Quick start (Windows 10/11)

1. Install **Python 3.11 (64-bit)** and make sure the Python Launcher (`py`) is available.
2. Install [Ollama](https://ollama.com/download/windows), then open PowerShell and run:
   ```powershell
   ollama pull qwen3.5:4b
   ```
3. Install [ComfyUI](https://docs.comfy.org/installation/desktop/windows) if you want image/video generation. Add at least one checkpoint to ComfyUI's `models/checkpoints` folder. The default image checkpoint name is `v1-5-pruned-emaonly.safetensors`.
4. Download or clone this repository.
5. Double-click `setup_windows.bat` once.
6. Double-click `start_jarvis.bat`.
7. Open **http://127.0.0.1:5000** in Chrome or Edge.

For detailed instructions, optional services, model setup, voice setup, and troubleshooting, read [SETUP_GUIDE.md](SETUP_GUIDE.md) and [TROUBLESHOOTING.md](TROUBLESHOOTING.md).

## Included features

- Ollama chat with streamed responses, model selection, elapsed time, token statistics, and cancellation.
- Browser chat history and settings (stored in that browser's local storage).
- Browser speech recognition and browser text-to-speech where supported.
- Local Piper voice catalog, voice downloads, and WAV synthesis.
- ComfyUI image generation, estimate history, gallery, and deletion controls.
- Optional ComfyUI video generation when a compatible API workflow and its models/custom nodes are installed.
- Optional online web search and Wikimedia Commons image search. Network tools start disabled.
- Text, code, image, and ZIP uploads for context; uploaded code is read as text and is not executed.
- A restricted workspace for editing/saving files and previewing HTML/CSS/JavaScript in a sandboxed iframe. It does **not** execute Python, C++, shell commands, or arbitrary programs.

## Project layout

```text
JARVIS/
├── app.py                  # Flask API and local-service integrations
├── index.html              # Web interface (HTML/CSS/JavaScript)
├── piper_worker.py         # Optional Piper subprocess fallback
├── requirements.txt        # Python dependencies
├── .env.example            # Portable local configuration template
├── setup_windows.bat       # One-time Python environment setup
├── start_jarvis.bat        # Starts the Flask server
├── static/                 # Optional static interface assets
├── piper_voices/           # Downloaded Piper voices (not committed)
├── generated_images/       # Runtime image output (not committed)
├── generated_videos/       # Runtime video output (not committed)
└── jarvis_workspace/       # User-created IDE files (not committed)
```

The app creates runtime folders automatically if they are missing. `image_benchmarks.json` and `video_workflow_api.json` are created only when needed.

## Configuration

`setup_windows.bat` creates `.env` from `.env.example`. Edit `.env` to change service URLs, the default Ollama model, or the local web-server port. `.env` is ignored by Git and should not contain passwords or API keys.

Default services:

| Service | Default address | Required? |
|---|---|---|
| J.A.R.V.I.S. web app | `http://127.0.0.1:5000` | Yes |
| Ollama | `http://127.0.0.1:11434` | Yes for AI chat |
| ComfyUI | `http://127.0.0.1:8188` | Only for image/video generation |
| Piper HTTP server | `http://127.0.0.1:5005` | Optional fallback; local Piper is preferred |

The Flask server binds to `127.0.0.1` by default so it is accessible only from the same computer. Do not change this to `0.0.0.0` unless you understand the LAN exposure and have configured appropriate network protections.

## Fresh-data behavior

This release starts without prior chat history, generated images/videos, benchmark records, or downloaded voice models. The frontend uses a new browser-storage namespace and performs a one-time cleanup of the previous JARVIS v3 chat record and attachment database on first load at the same local origin. This does not erase unrelated browser data.

Generated outputs and downloaded Piper models are excluded from Git. They remain on the computer where they are created unless the user deletes them manually.

## Hardware notes

Performance depends on the GPU, available VRAM/RAM, model, resolution, workflow, and ComfyUI nodes. The app does not bundle or guarantee any model. Start with a small Stable Diffusion checkpoint and 512×512 images on lower-VRAM GPUs. FLUX, XL checkpoints, and video workflows can require substantially more memory and time.

## Privacy and network behavior

- Ollama, Piper local synthesis, ComfyUI, and the workspace are intended to run locally.
- The UI starts in **OFFLINE** mode. Web search, web image search, and downloading Piper voices require switching to **ONLINE** mode.
- Browser speech recognition may use browser/vendor services depending on the browser; it is not guaranteed to be fully offline.
- Google Fonts are not required; the interface uses local system font fallbacks.
- Do not expose the Flask server to the public internet. There is no account/login layer in this project.

## Status

This is a cleaned and portable source release based on the files supplied for this rebuild. Python syntax and frontend JavaScript syntax can be checked locally, but Ollama, ComfyUI, microphone permissions, Piper audio playback, and GPU workflows must still be tested on the target computer.

# 🎙️ Text-to-Speech Experiments — Qwen3-TTS

A collection of 10 self-contained Google Colab notebooks for experimenting with **Qwen3-TTS** — Alibaba's state-of-the-art open-weight text-to-speech model series.

> Supports **Voice Cloning**, **Voice Design**, and **Custom Preset Voices** across 10 languages — fully free, no API key needed.

---

## 📋 Notebooks

| # | Notebook | Description | Model |
|---|---|---|---|
| 1 | [01_Audiobook_Studio_MVP.ipynb](./01_Audiobook_Studio_MVP.ipynb) | Long-form audiobook narration with consistent voice | `1.7B-Base` |
| 2 | [02_MultiSpeaker_Drama_MVP.ipynb](./02_MultiSpeaker_Drama_MVP.ipynb) | Multi-character podcast & drama from script format | `1.7B-CustomVoice` |
| 3 | [03_Voice_Clone_Studio_MVP.ipynb](./03_Voice_Clone_Studio_MVP.ipynb) | Zero-shot voice cloning workbench (3–15s reference) | `1.7B-Base` |
| 4 | [04_Voice_Designer_Foundry_MVP.ipynb](./04_Voice_Designer_Foundry_MVP.ipynb) | Prompt engineering studio for voice persona design | `1.7B-VoiceDesign` |
| 5 | [05_Multilingual_Dubber_MVP.ipynb](./05_Multilingual_Dubber_MVP.ipynb) | TTS across all 10 supported languages | `1.7B-CustomVoice` |
| 6 | [06_News_RSS_Podcast_MVP.ipynb](./06_News_RSS_Podcast_MVP.ipynb) | Fetch live RSS news → spoken morning briefing | `1.7B-VoiceDesign` |
| 7 | [07_PDF_Document_Reader_MVP.ipynb](./07_PDF_Document_Reader_MVP.ipynb) | Convert PDF / .md / .txt files to narrated audio | `1.7B-VoiceDesign` |
| 8 | [08_Game_NPC_Voice_MVP.ipynb](./08_Game_NPC_Voice_MVP.ipynb) | NPC voice foundry for games & tabletop RPGs | `1.7B-VoiceDesign` |
| 9 | [09_Colab_Webhook_API_MVP.ipynb](./09_Colab_Webhook_API_MVP.ipynb) | Colab as a live REST API (FastAPI + Cloudflare Tunnel) | `0.6B-CustomVoice` |
| 10 | [10_AllInOne_Gradio_App_MVP.ipynb](./10_AllInOne_Gradio_App_MVP.ipynb) | 5-tab Gradio web app with shareable public URL | Switchable |

Also includes the original reference notebook:
- [Qwen3_TTS_Colab.ipynb](./Qwen3_TTS_Colab.ipynb) — Complete reference notebook with all Qwen3-TTS features

---

## 🚀 Quick Start

1. Open any notebook in [Google Colab](https://colab.research.google.com)
2. Go to **Runtime → Change runtime type → T4 GPU** (free tier works)
3. Run cells top to bottom — each notebook is fully self-contained

### Requirements
All dependencies are installed automatically inside each notebook:
```bash
pip install qwen-tts soundfile gradio feedparser pypdf
```

---

## 🧠 Model Overview

| Model | Use Case | VRAM |
|---|---|---|
| `Qwen/Qwen3-TTS-12Hz-1.7B-CustomVoice` | Preset speakers (Ryan, Vivian, …) | ~8 GB |
| `Qwen/Qwen3-TTS-12Hz-1.7B-VoiceDesign` | Design a voice from a text prompt | ~8 GB |
| `Qwen/Qwen3-TTS-12Hz-1.7B-Base` | Zero-shot voice cloning | ~8 GB |
| `Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice` | Faster, lower VRAM preset voices | ~4 GB |
| `Qwen/Qwen3-TTS-12Hz-0.6B-Base` | Faster, lower VRAM cloning | ~4 GB |

---

## 🌐 Languages Supported

English · Chinese · Japanese · Korean · French · German · Spanish · Italian · Russian · Portuguese

---

## 📦 Resources

- [Qwen3-TTS GitHub](https://github.com/QwenLM/Qwen3-TTS)
- [HuggingFace Collection](https://huggingface.co/collections/Qwen/qwen3-tts)
- [Live HF Demo](https://huggingface.co/spaces/Qwen/Qwen3-TTS-Demo)
- [Technical Paper](https://arxiv.org/abs/2601.15621)

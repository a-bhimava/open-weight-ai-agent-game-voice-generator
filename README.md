# 🎙️ Text-to-Speech Experiments — Qwen3-TTS

A collection of 31 self-contained Google Colab notebooks for experimenting with **Qwen3-TTS** — Alibaba's state-of-the-art open-weight text-to-speech model series.

> Supports **Voice Cloning**, **Voice Design**, and **Custom Preset Voices** across 10 languages — fully free, no API key needed.

---

## ⚡ Use Qwen3-TTS in 5 Lines

Generate custom-designed voice audio in just **5 lines of Python**:

```python
import torch, soundfile as sf
from qwen_tts import Qwen3TTSModel
model = Qwen3TTSModel.from_pretrained("Qwen/Qwen3-TTS-12Hz-1.7B-VoiceDesign", device_map="cuda:0", dtype=torch.bfloat16)
wavs, sr = model.generate_voice_design("Hello, welcome to the future of voice AI!", language="English", instruct="warm, clear professional narrator")
sf.write("output.wav", wavs[0], sr)
```

---

## 📋 Notebooks — Series 1: General TTS MVPs

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

---

## 🎮 Notebooks — Series 2: Game NPC Voice Deep Dive

| # | Notebook | Description | Model |
|---|---|---|---|
| 11 | [11_RPG_Class_Voice_Pack_MVP.ipynb](./11_RPG_Class_Voice_Pack_MVP.ipynb) | 8 RPG classes × 3 event lines (greeting, combat, death) + party intro scene | `1.7B-VoiceDesign` |
| 12 | [12_Horror_Dark_Entity_Pack_MVP.ipynb](./12_Horror_Dark_Entity_Pack_MVP.ipynb) | 6 horror archetypes (ghost, demon, cult leader, possessed, undead, eldritch) | `1.7B-VoiceDesign` |
| 13 | [13_SciFi_Species_Pack_MVP.ipynb](./13_SciFi_Species_Pack_MVP.ipynb) | 6 sci-fi species (hivemind, rogue AI, cyborg, mutant, diplomat, machine god) | `1.7B-VoiceDesign` |
| 14 | [14_Companion_Voice_Set_MVP.ipynb](./14_Companion_Voice_Set_MVP.ipynb) | Full 41-line companion event library via Design → Clone pipeline | `1.7B-VoiceDesign` + `1.7B-Base` |
| 15 | [15_Boss_Battle_Monologue_MVP.ipynb](./15_Boss_Battle_Monologue_MVP.ipynb) | 4 bosses × 3-phase encounter (intro → taunts → death speech) + selection reel | `1.7B-VoiceDesign` |
| 16 | [16_Branching_Dialogue_Tree_MVP.ipynb](./16_Branching_Dialogue_Tree_MVP.ipynb) | Dialogue tree with player-choice branches, tree-aware filenames & JSON manifest | `1.7B-VoiceDesign` |
| 17 | [17_Dynamic_Emotion_Engine_MVP.ipynb](./17_Dynamic_Emotion_Engine_MVP.ipynb) | 8 emotions × 3 intensities = 24-clip reactive voice grid | `1.7B-VoiceDesign` |
| 18 | [18_Voice_Aging_System_MVP.ipynb](./18_Voice_Aging_System_MVP.ipynb) | Same character at young / adult / elder ages — life story arcs | `1.7B-VoiceDesign` |
| 19 | [19_Ambient_Crowd_Chatter_MVP.ipynb](./19_Ambient_Crowd_Chatter_MVP.ipynb) | 5 scene types → loopable ambient audio beds (market, tavern, battlefield, dungeon, space station) | `1.7B-CustomVoice` |
| 20 | [20_Procedural_NPC_Generator_MVP.ipynb](./20_Procedural_NPC_Generator_MVP.ipynb) | Genre seed → deterministic unique NPC (name + backstory + voice + lines) | `1.7B-VoiceDesign` |

---

## 🎬 Notebooks — Series 3: Character Voice Aging & Iconic Archetypes

| # | Notebook | Description | Model |
|---|---|---|---|
| 21 | [21_Character_Voice_Aging_MVP.ipynb](./21_Character_Voice_Aging_MVP.ipynb) | Complete aging system (8 archetypes × 4 life stages + life story montages) | `1.7B-VoiceDesign` |
| 22 | [22_Aria_Voss_MVP.ipynb](./22_Aria_Voss_MVP.ipynb) | Aria Voss: The Broken Empress (liberator → conqueror → tyrant → exile) | `1.7B-VoiceDesign` |
| 23 | [23_Zane_Holloway_MVP.ipynb](./23_Zane_Holloway_MVP.ipynb) | Zane Holloway: The Street Detective (rookie → ace → worn → legend) | `1.7B-VoiceDesign` |
| 24 | [24_Rex_Dagger_MVP.ipynb](./24_Rex_Dagger_MVP.ipynb) | Rex Dagger: The Cosmic Mercenary (reckless → reluctant hero → scarred → legend) | `1.7B-VoiceDesign` |
| 25 | [25_Nora_Vance_MVP.ipynb](./25_Nora_Vance_MVP.ipynb) | Nora Vance: The Vigilante Hacker (recluse → operative → ghost → vanished) | `1.7B-VoiceDesign` |
| 26 | [26_Theo_Ashworth_MVP.ipynb](./26_Theo_Ashworth_MVP.ipynb) | Theo Ashworth: The Fallen King (crowned → ruler → fallen → wanderer) | `1.7B-VoiceDesign` |
| 27 | [27_Cora_Langston_MVP.ipynb](./27_Cora_Langston_MVP.ipynb) | Cora Langston: The Time Traveler (explorer → veteran → weighted → serene) | `1.7B-VoiceDesign` |
| 28 | [28_Lena_Stavros_MVP.ipynb](./28_Lena_Stavros_MVP.ipynb) | Lena Stavros: The Battle Medic (idealist → surgeon → worn → veteran) | `1.7B-VoiceDesign` |
| 29 | [29_Dorian_Slate_MVP.ipynb](./29_Dorian_Slate_MVP.ipynb) | Dorian Slate: The Reluctant Prophet (ordinary → reluctant → carrier → transcendent) | `1.7B-VoiceDesign` |
| 30 | [30_Mara_Stone_MVP.ipynb](./30_Mara_Stone_MVP.ipynb) | Mara Stone: The Immortal Assassin (weapon → questioning → reclaiming → human) | `1.7B-VoiceDesign` |
| 31 | [31_Otto_Crane_MVP.ipynb](./31_Otto_Crane_MVP.ipynb) | Otto Crane: The Gruff Engineer (upstart → master → veteran → legend) | `1.7B-VoiceDesign` |

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

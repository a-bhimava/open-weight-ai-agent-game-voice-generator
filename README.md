# 🎙️ Chronovox

**A voice production system for game audio — built on Qwen3-TTS.**

Chronovox generates complete character voice packs for games: NPC dialogue libraries, branching conversation trees with engine-ready manifests, boss encounter arcs, emotional response grids — and characters whose voices **age credibly across a lifetime**.

Every module runs on a free Colab T4. No API key, no per-character billing, no rate limit.

> **The headline capability:** the same character, rendered at 17, 32, 50, and 68 — recognizably one person, audibly a different age. Not pitch-shifted. Re-synthesized, with the persona held invariant and only the age-carried qualities of the voice allowed to move.

---

## What It Produces

| Module | Output | Scale |
|---|---|---|
| **Companion event library** | Full NPC dialogue set at a single fixed voice identity | 41 lines across 11 event categories |
| **Character lifetime arcs** | One character across 4 life stages, with life-story montage | 11 characters × 4 stages |
| **Boss encounters** | Phase-structured fight audio (intro → taunts → desperation → death) | 4 bosses × 7 clips |
| **Branching dialogue** | Conversation graph + `dialogue_manifest.json` for engine playback | 11-node tree, depth 3 |
| **Emotion grid** | Reactive dialogue matrix for game-state-driven selection | 8 emotions × 3 intensities |
| **Class & species packs** | Archetype voice sets with per-event lines | 8 RPG classes, 6 horror entities, 6 sci-fi species |
| **Ambient crowd beds** | Loopable background barks for scene atmosphere | 5 scenes, 32 barks |
| **Procedural NPCs** | Seeded persona generation — name, backstory, voice, lines | Reproducible from an integer seed |

Plus infrastructure: a REST endpoint over Colab (FastAPI + Cloudflare Tunnel), a 5-tab Gradio console, and long-form audiobook and document narration.

---

## Quickstart

```python
import torch, soundfile as sf
from qwen_tts import Qwen3TTSModel
model = Qwen3TTSModel.from_pretrained("Qwen/Qwen3-TTS-12Hz-1.7B-VoiceDesign", device_map="cuda:0", dtype=torch.bfloat16)
wavs, sr = model.generate_voice_design("Hello, welcome to the future of voice AI!", language="English", instruct="warm, clear professional narrator")
sf.write("output.wav", wavs[0], sr)
```

1. Open any notebook in [Google Colab](https://colab.research.google.com)
2. **Runtime → Change runtime type → T4 GPU** (free tier is sufficient)
3. Run top to bottom — every notebook is self-contained

```bash
pip install qwen-tts soundfile gradio feedparser pypdf
```

---

## Architecture: Two Modes, Chosen Deliberately

The central design decision in this system is knowing **which generation mode a given job needs**. Qwen3-TTS offers two, and they are not interchangeable.

### Mode 1 — VoiceDesign, for *variation*

```python
wavs, sr = model.generate_voice_design(text=line, language="English", instruct=persona_prompt)
```

Each call is an independent sample conditioned on a natural-language persona prompt. Two calls with the same prompt produce two takes that are tonally consistent but not identical. **That is the correct behavior when clips are meant to differ** — an emotion grid, a species roster, a procedurally generated NPC crowd.

### Mode 2 — Design → Clone, for *identity*

```python
# 1. Design the voice once, and persist it as an audio blueprint
ref = vd_model.generate_voice_design(text=REF_SENTENCE, language="English", instruct=persona_prompt)

# 2. Compile that blueprint into a reusable clone prompt
prompt = base_model.create_voice_clone_prompt(ref_audio=ref_path, ref_text=REF_SENTENCE)

# 3. Render unlimited lines at a pinned identity
for line in event_library:
    wavs, sr = base_model.generate_voice_clone(text=line, language="English", voice_clone_prompt=prompt)
```

One design call fixes the timbre; every subsequent line inherits it exactly. **This is what any character speaking more than a couple of consecutive lines requires**, because per-line drift that is invisible in a scattered voice pack becomes audible mid-conversation.

Reference implementation: [`14_Companion_Voice_Set_MVP.ipynb`](./02_game_npc_voices/14_Companion_Voice_Set_MVP.ipynb) — one design call, 41 cloned lines. The same pipeline carries timbre *across languages* in [`05_Multilingual_Dubber_MVP.ipynb`](./01_general_tts/05_Multilingual_Dubber_MVP.ipynb): design in English, render in Chinese, French, and Japanese with the voice preserved.

---

## 📂 Repository Structure

```
chronovox/
├── 01_general_tts/           # Notebooks 01–10 — core synthesis & delivery
├── 02_game_npc_voices/       # Notebooks 11–20 — game & NPC production modules
├── 03_character_archetypes/  # Notebooks 21–31 — lifetime voice modulation
└── reference/                # Full Qwen3-TTS API reference notebook
```

| Folder | Notebooks | What's Inside | Start Here If You Want To… |
|---|---|---|---|
| **[`01_general_tts/`](./01_general_tts/)** | 01–10 | Audiobooks, multi-speaker drama, voice cloning, voice design, multilingual dubbing, RSS/PDF readers, a REST API, and an all-in-one Gradio console | Learn the primitives, clone a voice, or stand up a general-purpose TTS service |
| **[`02_game_npc_voices/`](./02_game_npc_voices/)** | 11–20 | RPG class packs, horror & sci-fi archetypes, companion voice sets, boss monologues, branching dialogue trees, emotion grids, crowd ambience, procedural NPCs | Bulk-generate game dialogue, NPC voice packs, or reactive game audio |
| **[`03_character_archetypes/`](./03_character_archetypes/)** | 21–31 | One deep-dive module per named character — each aging across 4 life stages with full narrative arcs, plus a general 8-archetype aging system | Build a recurring character whose voice evolves over their lifetime |
| **[`reference/`](./reference/)** | — | The complete API surface, unwrapped by any application logic | Look up a method, parameter, or capability directly |

> **New here?** Start with [`01_Audiobook_Studio_MVP.ipynb`](./01_general_tts/01_Audiobook_Studio_MVP.ipynb) for the simplest end-to-end path, or [`10_AllInOne_Gradio_App_MVP.ipynb`](./01_general_tts/10_AllInOne_Gradio_App_MVP.ipynb) to drive every feature from a UI.

---

## 📋 Series 1 — Core Synthesis & Delivery

| # | Module | Description | Model |
|---|---|---|---|
| 1 | [01_Audiobook_Studio_MVP.ipynb](./01_general_tts/01_Audiobook_Studio_MVP.ipynb) | Long-form narration at a fixed cloned voice, with chunked assembly | `1.7B-Base` |
| 2 | [02_MultiSpeaker_Drama_MVP.ipynb](./01_general_tts/02_MultiSpeaker_Drama_MVP.ipynb) | Multi-character drama from a `[Speaker]: line` script format | `1.7B-CustomVoice` |
| 3 | [03_Voice_Clone_Studio_MVP.ipynb](./01_general_tts/03_Voice_Clone_Studio_MVP.ipynb) | Zero-shot cloning workbench — URL, upload, or local reference | `1.7B-Base` |
| 4 | [04_Voice_Designer_Foundry_MVP.ipynb](./01_general_tts/04_Voice_Designer_Foundry_MVP.ipynb) | Attribute-driven persona compiler with A/B/C audition | `1.7B-VoiceDesign` |
| 5 | [05_Multilingual_Dubber_MVP.ipynb](./01_general_tts/05_Multilingual_Dubber_MVP.ipynb) | All 10 languages, plus cross-lingual timbre transfer | `1.7B-CustomVoice` + `Base` |
| 6 | [06_News_RSS_Podcast_MVP.ipynb](./01_general_tts/06_News_RSS_Podcast_MVP.ipynb) | Live RSS ingest → spoken briefing | `1.7B-VoiceDesign` |
| 7 | [07_PDF_Document_Reader_MVP.ipynb](./01_general_tts/07_PDF_Document_Reader_MVP.ipynb) | PDF / Markdown / text → narrated audio with pre-flight estimation | `1.7B-VoiceDesign` |
| 8 | [08_Game_NPC_Voice_MVP.ipynb](./01_general_tts/08_Game_NPC_Voice_MVP.ipynb) | NPC roster with emotion modifiers and engine naming contract | `1.7B-VoiceDesign` + `Base` |
| 9 | [09_Colab_Webhook_API_MVP.ipynb](./01_general_tts/09_Colab_Webhook_API_MVP.ipynb) | Colab as a live REST API (FastAPI + Cloudflare Tunnel) | `0.6B-CustomVoice` + `1.7B-VoiceDesign` |
| 10 | [10_AllInOne_Gradio_App_MVP.ipynb](./01_general_tts/10_AllInOne_Gradio_App_MVP.ipynb) | 5-tab Gradio console with shareable public URL | Switchable |

## 🎮 Series 2 — Game Production Modules

| # | Module | Description | Model |
|---|---|---|---|
| 11 | [11_RPG_Class_Voice_Pack_MVP.ipynb](./02_game_npc_voices/11_RPG_Class_Voice_Pack_MVP.ipynb) | 8 RPG classes × 3 event lines + party intro reel | `1.7B-VoiceDesign` |
| 12 | [12_Horror_Dark_Entity_Pack_MVP.ipynb](./02_game_npc_voices/12_Horror_Dark_Entity_Pack_MVP.ipynb) | 6 horror archetypes, with derived whisper variants | `1.7B-VoiceDesign` |
| 13 | [13_SciFi_Species_Pack_MVP.ipynb](./02_game_npc_voices/13_SciFi_Species_Pack_MVP.ipynb) | 6 sci-fi species + scripted first-contact scene | `1.7B-VoiceDesign` |
| 14 | [14_Companion_Voice_Set_MVP.ipynb](./02_game_npc_voices/14_Companion_Voice_Set_MVP.ipynb) | **Design→Clone reference implementation** — 41-line event library at one identity | `1.7B-VoiceDesign` + `Base` |
| 15 | [15_Boss_Battle_Monologue_MVP.ipynb](./02_game_npc_voices/15_Boss_Battle_Monologue_MVP.ipynb) | 4 bosses × 3-phase encounter with cinematic pacing | `1.7B-VoiceDesign` |
| 16 | [16_Branching_Dialogue_Tree_MVP.ipynb](./02_game_npc_voices/16_Branching_Dialogue_Tree_MVP.ipynb) | Player-choice dialogue graph + JSON manifest | `1.7B-VoiceDesign` |
| 17 | [17_Dynamic_Emotion_Engine_MVP.ipynb](./02_game_npc_voices/17_Dynamic_Emotion_Engine_MVP.ipynb) | 8 emotions × 3 intensities = 24-clip reactive grid | `1.7B-VoiceDesign` |
| 18 | [18_Voice_Aging_System_MVP.ipynb](./02_game_npc_voices/18_Voice_Aging_System_MVP.ipynb) | 3-stage aging prototype across 3 characters | `1.7B-VoiceDesign` |
| 19 | [19_Ambient_Crowd_Chatter_MVP.ipynb](./02_game_npc_voices/19_Ambient_Crowd_Chatter_MVP.ipynb) | 5 scene types → loopable ambient beds | `1.7B-CustomVoice` |
| 20 | [20_Procedural_NPC_Generator_MVP.ipynb](./02_game_npc_voices/20_Procedural_NPC_Generator_MVP.ipynb) | Genre seed → unique NPC (name, backstory, voice, lines) | `1.7B-VoiceDesign` |

## 🎬 Series 3 — Lifetime Voice Modulation

Each module renders one character across four life stages — **Youth (16–22) → Prime (28–36) → Middle (46–55) → Elder (65–78)** — with three lines per stage and, where applicable, a life-story montage and an emotional range set.

| # | Module | Character Arc | Model |
|---|---|---|---|
| 21 | [21_Character_Voice_Aging_MVP.ipynb](./03_character_archetypes/21_Character_Voice_Aging_MVP.ipynb) | The aging system itself — 8 archetypes × 4 stages, with cross-character comparison reels | `1.7B-VoiceDesign` |
| 22 | [22_Aria_Voss_MVP.ipynb](./03_character_archetypes/22_Aria_Voss_MVP.ipynb) | Aria Voss, The Broken Empress — liberator → conqueror → tyrant → exile | `1.7B-VoiceDesign` |
| 23 | [23_Zane_Holloway_MVP.ipynb](./03_character_archetypes/23_Zane_Holloway_MVP.ipynb) | Zane Holloway, The Street Detective — rookie → ace → worn → legend | `1.7B-VoiceDesign` |
| 24 | [24_Rex_Dagger_MVP.ipynb](./03_character_archetypes/24_Rex_Dagger_MVP.ipynb) | Rex Dagger, The Cosmic Mercenary — reckless → reluctant hero → scarred → legend | `1.7B-VoiceDesign` |
| 25 | [25_Nora_Vance_MVP.ipynb](./03_character_archetypes/25_Nora_Vance_MVP.ipynb) | Nora Vance, The Vigilante Hacker — recluse → operative → ghost → vanished | `1.7B-VoiceDesign` |
| 26 | [26_Theo_Ashworth_MVP.ipynb](./03_character_archetypes/26_Theo_Ashworth_MVP.ipynb) | Theo Ashworth, The Fallen King — crowned → ruler → fallen → wanderer | `1.7B-VoiceDesign` |
| 27 | [27_Cora_Langston_MVP.ipynb](./03_character_archetypes/27_Cora_Langston_MVP.ipynb) | Cora Langston, The Time Traveler — explorer → veteran → weighted → serene | `1.7B-VoiceDesign` |
| 28 | [28_Lena_Stavros_MVP.ipynb](./03_character_archetypes/28_Lena_Stavros_MVP.ipynb) | Lena Stavros, The Battle Medic — idealist → surgeon → worn → veteran | `1.7B-VoiceDesign` |
| 29 | [29_Dorian_Slate_MVP.ipynb](./03_character_archetypes/29_Dorian_Slate_MVP.ipynb) | Dorian Slate, The Reluctant Prophet — ordinary → reluctant → carrier → transcendent | `1.7B-VoiceDesign` |
| 30 | [30_Mara_Stone_MVP.ipynb](./03_character_archetypes/30_Mara_Stone_MVP.ipynb) | Mara Stone, The Immortal Assassin — weapon → questioning → reclaiming → human | `1.7B-VoiceDesign` |
| 31 | [31_Otto_Crane_MVP.ipynb](./03_character_archetypes/31_Otto_Crane_MVP.ipynb) | Otto Crane, The Gruff Engineer — upstart → master → veteran → legend | `1.7B-VoiceDesign` |

---

## 🧠 Model Selection

| Model | Role | VRAM* |
|---|---|---|
| `Qwen/Qwen3-TTS-12Hz-1.7B-VoiceDesign` | Persona synthesis from a text prompt — **1.7B only, no 0.6B variant** | ~8 GB |
| `Qwen/Qwen3-TTS-12Hz-1.7B-Base` | Zero-shot cloning and clone-prompt rendering | ~8 GB |
| `Qwen/Qwen3-TTS-12Hz-1.7B-CustomVoice` | Preset speakers (Ryan, Vivian, …) with instruct-driven delivery | ~8 GB |
| `Qwen/Qwen3-TTS-12Hz-0.6B-CustomVoice` | Lower-VRAM preset voices — used to co-reside with VoiceDesign in the API module | ~4 GB |
| `Qwen/Qwen3-TTS-12Hz-0.6B-Base` | Lower-VRAM cloning | ~4 GB |

\* Vendor/spec-derived figures for capacity planning. This repository does not ship measured benchmarks — see [Capability Evaluation](#-capability-evaluation).

### Technical Specifications

| Parameter | Specification | Notes |
|---|---|---|
| **Architecture** | LM-based TTS on the Qwen 3 backbone | Dedicated audio tokenization |
| **Audio token rate** | `12 Hz` | This is the token frame rate — **not** the output sample rate |
| **Output sample rate** | `24 kHz` | All modules write at 24 kHz |
| **Model sizes** | `1.7B`, `0.6B` | 1.7B for production quality; 0.6B for VRAM-constrained or co-resident use |
| **Cloning reference** | `3–15 seconds` | Clean single-speaker audio plus its transcript |
| **Languages** | `10` | English, Chinese, Japanese, Korean, German, French, Spanish, Italian, Russian, Portuguese |
| **Precision** | `bfloat16` / `float16` / `float32` | All modules run `bfloat16` + `sdpa` attention |

---

## 🔬 Capability Evaluation

Before committing to a direction, we exercised the full technical surface of Qwen3-TTS across twenty production modules — one capability per module, each pushed to a realistic content volume rather than a one-line demo. What follows is what that surface actually supports, and where each capability belongs.

**These are architectural findings, derived from building against every mode. This repository ships no latency, MOS, or throughput benchmarks — none were measured, and none are claimed.**

| Capability | Verdict | Finding |
|---|---|---|
| **Preset speakers + instruct** | Production-ready | `instruct` is *orthogonal* to `speaker` — the same preset timbre takes direction independently. Two preset voices carried 32 distinct characters in the ambient crowd module. |
| **Zero-shot cloning** | Production-ready | A 3–15s reference plus its transcript reproduces identity, tone, and acoustic character. Compiling the reference into a clone prompt once and reusing it is the cheap path for bulk rendering. |
| **Design → Clone** | The consistency primitive | One design call fixes a voice; unlimited lines inherit it. This is the only mechanism in the stack that pins identity across a large line count. |
| **Per-line VoiceDesign** | Variation only | Each call is an independent sample. Timbre is not pinned between calls, so it is right for rosters and grids, wrong for consecutive dialogue. |
| **Cross-lingual transfer** | Works, via Design→Clone | An English-designed voice carries into Chinese, French, and Japanese with timbre preserved — the model separates speaker identity from language. |
| **Emotional control** | Prompt-native, no numeric knob | Intensity is expressed in language, not parameters. Holding text constant across the 24-clip grid isolates prompt effect as a controlled comparison. |
| **Branching dialogue** | Structurally solved, identity-fragile | Node IDs double as dict key, filename stem, and manifest key, so an engine resolves a choice to an audio file with zero mapping logic. The 11 nodes still need Design→Clone to stay one voice. |
| **Procedural generation** | Deterministic personas | A seed reproduces the name, backstory, prompt, and line exactly. Note that this fixes the *persona*, not the rendered waveform. |
| **Serving** | Viable for prototyping | A 0.6B preset model and 1.7B VoiceDesign co-reside in T4 VRAM behind FastAPI, streaming WAVs from memory with no temp files. |
| **Ambient crowds** | Effective, mechanically simple | Sequential barks with randomized spacing — concatenation, not mixing. The characterization work is done entirely by `instruct` diversity. |

The consistent thread: **Qwen3-TTS's expressive range lives in natural language, not in parameters.** There is no age slider, no emotion dial, no rasp coefficient. Everything is a prompt. That constraint is what made the next decision obvious.

---

## 🎯 Why We Built Around Age-Based Modulation

Of everything above, one capability had no substitute anywhere else — and it happened to be the one the prompt-native architecture is uniquely good at.

**1. Every other capability was purchasable. Lifetime continuity was not.**
Cloning, preset voices, emotional delivery, multilingual output — all are commodity features of hosted TTS platforms. Voice *over time* is not sold by anyone. A studio that wants a protagonist heard at 19 and again at 64 has to build it.

**2. Aging is where natural-language design beats parameter synthesis outright.**
The dimensions that actually signal age are not acoustic parameters. They are *"gravel replacing polish"*, *"wisdom showing in the pauses"*, *"the fire entirely gone"*, *"precision still there but serving warmth now rather than suppression"*. No slider expresses those. And the naive DSP approach — pitch-shifting a young voice down — produces an artifact, not a person: it moves the fundamental while leaving pace, breath, and articulation untouched, which is precisely the set of things that age. A prompt-native model can move all of them at once because they were never separate parameters to begin with.

**3. It composes exactly with the architecture we already had.**
The aging construction is an invariant `personality_core` — who the character *is*, never edited — concatenated with a per-stage modifier that carries only what age changed:

```python
instruct = f"{personality_core}, {stage['voice_modifier']}"
```

The invariant stem is what keeps four life stages recognizably one person; the modifier is what makes them audibly different ages. And because the stem is already isolated, Design→Clone drops in cleanly on top: design each stage once, pin it, then render that stage's entire line set at a locked identity.

**4. It is what a narrative game genuinely cannot buy.**
Flashbacks, time-skips, generational sagas, a companion who ages across a campaign, a villain heard young in one act and old in the next — these are ordinary narrative structures with no off-the-shelf voice solution. That gap is the reason this project exists.

---

## ⚖️ Why Open Weights

ElevenLabs is the quality benchmark, and we treat it as such. The question was never which model sounds better in a single clip — it was whether a hosted, metered API can support this workload at all.

| | Chronovox (Qwen3-TTS) | ElevenLabs |
|---|---|---|
| **Lifetime voice continuity** | Core capability | Not offered |
| **Identity pinning** | Explicit, via compiled clone prompts | Implicit, per-voice |
| **Bulk generation** | Unmetered — compute is the only cost | Per-character credits |
| **Licensing** | Open weights, run anywhere | Hosted API only |
| **Privacy** | Fully offline; audio never leaves your machine | Processed on vendor servers |
| **Customization** | Weights, precision, batching, fine-tuning | Exposed API parameters |
| **Languages** | 10 | 30+ |
| **Single-clip polish** | Very strong | Generally the benchmark |
| **Setup cost** | A GPU and a notebook | An API key |

A 41-line companion library is one character. A full cast is thousands of lines, regenerated every time the writing changes. Metered synthesis prices that iteration loop out of reach, and no hosted API exposes the identity-pinning primitive the work depends on. **Open weights aren't a budget compromise here — they're what makes the workload possible.**

Where ElevenLabs wins, it wins clearly: breadth of languages, zero operational burden, and the best single-clip polish available. For a small cast in a widely-localized product, it remains the right call.

---

## 🗺️ Roadmap

Planned, not yet shipped:

- **Batched generation.** The API accepts list-valued `text`, `language`, and `instruct`; every module currently renders serially in a Python loop. This is the largest available speedup.
- **`x_vector_only_mode`** as an explicit speed/quality control for bulk packs.
- **Design→Clone across the lifetime modules**, pinning each life stage the way the companion library pins its single voice.
- **True crowd layering** — overlapping and gain-staging ambient barks rather than concatenating them.
- **Standardized JSON manifests** across every module, matching the dialogue tree's engine-ready format.
- **A measured benchmark suite** — real latency, VRAM, and batch-throughput figures to replace the spec-derived numbers above.

---

## 📦 Resources

- [Qwen3-TTS GitHub](https://github.com/QwenLM/Qwen3-TTS)
- [HuggingFace Collection](https://huggingface.co/collections/Qwen/qwen3-tts)
- [Live HF Demo](https://huggingface.co/spaces/Qwen/Qwen3-TTS-Demo)
- [Technical Paper](https://arxiv.org/abs/2601.15621)

Model weights are governed by their upstream Qwen licenses; review them before commercial use.

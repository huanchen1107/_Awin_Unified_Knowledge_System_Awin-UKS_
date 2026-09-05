# HISTORICAL RESEARCH — No-Quark Reproducible Digital Human Stack

**Date:** 2026-09-05  
**Status:** Historical research source; technical findings retained  
**Important:** This document is **not** OpenSpec Change 003. Canonical Change 003 is `agent-runtime-execution-fabric`.

Canonical current planning:

- `_myplan_/discussions/2026-09-05_Awin-CEO-Executive-Presence-Digital-Human_Architecture-Discussion.md`
- `_myplan_/decisions/2026-09-05_Awin-CEO-Executive-Presence_Planning-Boundary-and-Stack.md`

Antigravity must use the manifest/registry and repository inspection before assigning this capability to an OpenSpec Change.

---

## Research Conclusion

The Freedidi tutorial is useful as an implementation reference, but the Awin CEO Digital Human stack should **not depend on Quark Cloud / 夸克網盤** or on an opaque third-party one-click bundle.

The major runtime components can be obtained from official or reproducible sources:

- `ggml-org/llama.cpp`
- Qwen GGUF models from Hugging Face
- Hugging Face `speech-to-speech`
- `faster-whisper`
- official Qwen3-TTS implementation/model sources
- `lipku/LiveTalking`
- LiveTalking Wav2Lip model files from the project's documented alternative download source
- MuseTalk 1.5 via official repository/download scripts
- ComfyUI via official distribution
- Wan2.2 via official model/workflow distribution paths

The Freedidi-specific glue scripts should be **reimplemented rather than copied**:

```text
install-voice.sh
start-voice.sh
start-livetalking.sh
avatar-sync.js
```

These responsibilities should become first-party Awin bootstrap/runtime services.

## Reproducibility Principle

```text
Official / traceable sources
        +
reproducible install
        +
provider-neutral adapters
        +
private voice/avatar assets outside Git
```

Quark Cloud may be used manually for research, but must never be required by the official Awin bootstrap path.

## Recommended Runtime Candidate Stack

```text
Chairman
   │
Microphone / Text
   ▼
VAD
   ▼
faster-whisper / STT Provider
   ▼
Awin Presence Gateway
   ▼
Awin CEO Core
   ├── UKS retrieval
   └── LLM Provider
          ├── llama.cpp + local Qwen
          └── approved alternate provider
   ▼
Qwen3-TTS / TTS Provider
   ▼
LiveTalking / Avatar Provider
   ├── Wav2Lip      low-VRAM single-GPU candidate
   └── MuseTalk 1.5 high-quality candidate
   ▼
WebRTC
   ▼
Awin Executive Cockpit
```

## Single-GPU Finding

The detailed Freedidi example indicates a practical 24 GB class GPU profile can allocate approximately:

```text
Local LLM            ~9 GB
STT / Whisper         ~3 GB
Qwen3-TTS             ~4 GB
Wav2Lip               ~1.3 GB
---------------------------
Total                 ~17 GB approximate
```

Therefore Wav2Lip is a strong default candidate for the first 24 GB single-GPU profile, while MuseTalk 1.5 remains the higher-quality option when GPU headroom or a dedicated avatar GPU exists.

These figures are reference estimates and must be benchmarked by Antigravity on the actual target environment before becoming implementation contracts.

## Model / Software Source Policy

```yaml
model_registry:
  policy:
    quark_dependency: forbidden
    manual_download_required: false
    official_or_traceable_source_required: true

  preferred_sources:
    - github_release_or_repo
    - huggingface
    - modelscope
    - official_download_script
    - google_drive_documented_by_upstream
```

Record source, revision, license, local path and checksum when practical.

## Runtime vs Asset-Creation Separation

ComfyUI and Wan2.2 belong to avatar asset preparation, not normal runtime:

```text
AI-generated Awin portrait
        ↓
ComfyUI + Wan2.2 Image-to-Video
        ↓
awin_idle_v1.mp4
        ↓
Avatar preprocessing
        ↓
awin_ceo_v1
```

## Voice Profile Direction

Use structured private configuration rather than editing shell variables:

```yaml
voice_profiles:
  awin_ceo_v1:
    provider: qwen3_tts
    mode: zero_shot_clone
    reference_audio: private-assets/voices/awin/ref.wav
    reference_text_file: private-assets/voices/awin/ref.txt
    language: zh-TW
```

## Streaming-First Requirement

Target:

```text
LLM token stream
      ↓
phrase / sentence chunker
      ↓
streaming TTS
      ↓
audio chunks
      ↓
Avatar Provider
      ↓
WebRTC
```

Support interruption / barge-in and cancellation across the entire media path.

## Bootstrap Direction

Target operator experience may evolve toward:

```bash
git clone <repo>
cd <repo>
./scripts/bootstrap.sh
awin doctor
awin start
```

or equivalent managed container/supervisor workflow.

`doctor` should check GPU/VRAM, approved model artifacts, ports, WebRTC/ICE, microphone secure context, WSL-specific prerequisites where applicable, service health and private asset configuration.

## WSL / Windows Lessons

Preserve these as diagnostics, not tribal knowledge:

- mirrored networking/host loopback may matter for topology;
- avoid conflicting Linux NVIDIA display-driver installation under WSL GPU passthrough;
- native WSL filesystem may outperform `/mnt/c` for latency-sensitive workloads;
- normalize CRLF shell scripts;
- detect Hyper-V excluded port ranges;
- verify WebRTC ICE/STUN configuration;
- browser microphone access requires localhost or secure context.

## Degraded Modes

```text
Avatar unavailable → voice + cockpit
TTS unavailable    → text cockpit
STT unavailable    → typed input
local LLM failure  → approved alternate runtime/provider
UKS unavailable    → explicitly report evidence retrieval unavailable
```

## Governance Note

These are planning findings only. They must not be converted directly into OpenSpec artifacts by ChatGPT. Antigravity must resolve how this capability relates to existing roadmap Changes 003, 005 and 006 before formalization.

# Change 003 Research — No-Quark Reproducible Digital Human Stack

**Date:** 2026-09-05  
**Scope:** Research companion for `003-awin-ceo-digital-human-executive-interface`  
**Status:** Recommended architecture update

---

## 1. Research Conclusion

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

The only items from the Freedidi package that must be **reimplemented rather than copied** are its custom glue scripts:

```text
install-voice.sh
start-voice.sh
start-livetalking.sh
avatar-sync.js
```

These should be replaced by first-party Awin bootstrap/runtime services.

---

## 2. Architectural Principle

> Rebuild the workflow; do not preserve the opaque package.

Required invariant:

```text
Official / traceable sources
        +
reproducible install
        +
provider-neutral adapters
        +
private voice/avatar assets outside Git
```

Quark Cloud may be used manually by a human for research, but it must never be a required dependency of the official Awin bootstrap path.

---

## 3. Recommended Runtime Stack

```text
Chairman
   │
Microphone / Text
   │
   ▼
VAD
   │
   ▼
faster-whisper / STT Provider
   │
   ▼
Awin Presence Gateway
   │
   ▼
Awin CEO Core
   │
   ├── UKS CIO retrieval
   └── LLM Provider
          ├── llama.cpp + local Qwen
          └── cloud provider as optional profile
   │
   ▼
Qwen3-TTS / TTS Provider
   │
streaming audio
   │
   ▼
LiveTalking / Avatar Provider
   │
   ├── Wav2Lip      default low-VRAM profile
   └── MuseTalk 1.5 high-quality profile
   │
   ▼
WebRTC
   │
   ▼
Awin Executive Cockpit
```

---

## 4. Why Wav2Lip Becomes the Default for the Single-GPU Profile

The detailed Freedidi configuration showed an important resource constraint:

```text
Example 24 GB GPU profile

Local LLM            ~9 GB
STT / Whisper         ~3 GB
Qwen3-TTS             ~4 GB
Wav2Lip               ~1.3 GB
---------------------------
Total                 ~17 GB approximate
```

This leaves enough headroom for a practical single-GPU workstation profile.

Therefore the recommended Change 003 priority is revised:

```yaml
avatar_engine_profiles:
  single_gpu_default:
    provider: livetalking
    lip_sync: wav2lip

  high_quality:
    provider: livetalking
    lip_sync: musetalk_1_5
```

MuseTalk remains strongly recommended where GPU headroom is available or where the avatar worker runs on a dedicated GPU.

---

## 5. Model / Software Source Policy

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

Every model should be recorded with:

```yaml
model:
  id: qwen3_tts_base
  provider: qwen
  source_type: huggingface
  source: <official-or-approved-source>
  revision: <pin>
  checksum: <sha256-when-practical>
  license: <recorded-license>
  local_path: models/qwen3-tts/
```

The production bootstrap should pin revisions rather than always pulling latest.

---

## 6. Freedidi Package Mapping

| Freedidi Component | Awin Replacement |
|---|---|
| `install-voice.sh` | `scripts/bootstrap/install-speech.sh` + dependency manifest |
| `start-voice.sh` | managed Speech Runtime service |
| `start-livetalking.sh` | managed Avatar Worker service |
| `avatar-sync.js` | Presence Gateway / media bridge |
| bundled `wav2lip256.pth` | upstream-traceable model download |
| bundled demo avatar | upstream-traceable demo asset only for smoke test |
| shell `REF_TEXT` editing | versioned config + private voice profile |
| three terminal startup | supervisor / compose / `awin start` |

Do not copy unknown glue code into the core architecture merely because it appears in a one-click package.

---

## 7. Runtime vs Asset-Creation Separation

ComfyUI and Wan2.2 should not become runtime dependencies.

They belong to an **Avatar Asset Preparation** workflow:

```text
AI-generated Awin portrait
        ↓
ComfyUI + Wan2.2 Image-to-Video
        ↓
awin_idle_v1.mp4
        ↓
LiveTalking avatar preparation
        ↓
awin_ceo_v1
```

After the avatar has been prepared, the runtime stack should not require ComfyUI or Wan2.2.

---

## 8. Voice Profile Design

Replace shell-level voice configuration with a proper private profile:

```yaml
voice_profiles:
  awin_ceo_v1:
    provider: qwen3_tts
    mode: zero_shot_clone
    reference_audio: private-assets/voices/awin/ref.wav
    reference_text_file: private-assets/voices/awin/ref.txt
    language: zh-TW
```

Rules:

- raw voice media is private;
- do not commit it to public Git;
- reference transcript must match the audio exactly where the selected TTS model requires it;
- allow versioned voice profiles;
- allow provider replacement.

---

## 9. Streaming-First Requirement

The tutorial accepts approximately 1–2 seconds first-response latency and notes that full speech may be synthesized before lip-sync delivery.

Awin should instead target:

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

Important design requirements:

- do not wait for a complete multi-sentence answer before starting TTS;
- support interruption / barge-in;
- cancel pending TTS chunks when Chairman interrupts;
- propagate cancellation to avatar playback;
- keep spoken output concise by default.

---

## 10. Suggested Bootstrap Experience

Target user experience:

```bash
git clone <repo>
cd <repo>
./scripts/bootstrap.sh
awin doctor
awin start
```

or:

```bash
docker compose up
```

The bootstrap process should:

1. detect GPU and available VRAM;
2. select or recommend a runtime profile;
3. download pinned model artifacts from approved sources;
4. verify checksums where possible;
5. create isolated Python environments or containers;
6. validate microphone/WebRTC prerequisites;
7. configure private asset directories;
8. run smoke tests;
9. produce a final health report.

Example health output:

```text
Awin Core          READY
UKS                READY
LLM                READY
STT                READY
TTS                READY
Avatar             READY
WebRTC             READY
Executive Cockpit  READY
```

---

## 11. Hardware Profiles

### Profile A — 24 GB Single GPU

Recommended:

```yaml
llm: qwen-local-quantized
stt: faster-whisper
stt_size: medium_or_large_v3_based_on_headroom
tts: qwen3_tts
avatar: livetalking
lip_sync: wav2lip
```

### Profile B — 12–16 GB GPU

Recommended reductions:

```text
smaller quantized LLM
smaller STT model
Wav2Lip
shorter LLM context
```

### Profile C — Dedicated Avatar GPU / High Memory

Recommended:

```yaml
avatar: livetalking
lip_sync: musetalk_1_5
quality: high
```

### Profile D — Hybrid

```text
local avatar/STT/TTS
+
remote LLM
```

or the inverse where business/security requirements allow.

---

## 12. WSL / Windows Notes

The Freedidi procedure highlights several practical WSL2 concerns that should become diagnostics rather than tribal knowledge:

- WSL mirrored networking may be required for the chosen topology;
- host-loopback behavior should be checked automatically;
- do not install conflicting Linux NVIDIA display drivers inside WSL when Windows GPU passthrough is being used;
- avoid running latency-sensitive workloads directly from `/mnt/c` when native WSL storage performs materially better;
- detect Windows CRLF shell scripts and normalize during bootstrap;
- detect excluded Hyper-V port ranges before binding services;
- verify WebRTC ICE/STUN configuration;
- browser microphone access requires secure context / localhost rules.

These should become `awin doctor` checks.

---

## 13. Service Management

Do not keep the tutorial's permanent 'three terminal windows' model.

Recommended logical services:

```text
awin-core
uks
llm-worker
speech-runtime
avatar-worker
presence-gateway
executive-cockpit
```

They can initially be launched by scripts, but the architecture should converge on a managed runtime with:

- lifecycle management;
- health checks;
- logs;
- restart policy;
- dependency ordering;
- clean shutdown;
- degraded mode.

---

## 14. Degraded Modes

Awin must remain usable if a high-cost media subsystem fails.

```text
Avatar unavailable
→ voice + cockpit still works

TTS unavailable
→ text cockpit still works

Local LLM unavailable
→ approved alternate LLM provider may be used

UKS unavailable
→ Awin must state that enterprise evidence retrieval is unavailable
```

The digital human layer must never become a single point of failure for the CEO agent.

---

## 15. Acceptance Criteria Additions for Change 003

Add the following acceptance criteria:

1. Official Awin installation does not require Quark Cloud.
2. Every required runtime dependency has an approved reproducible source.
3. Model revisions are pinnable and recorded.
4. Private voice/avatar media is excluded from public Git history.
5. Single-GPU profile defaults to the lower-VRAM Wav2Lip path unless benchmarks justify otherwise.
6. MuseTalk 1.5 is available as a high-quality profile.
7. ComfyUI/Wan2.2 are setup-time tools, not production runtime dependencies.
8. Voice runtime configuration uses structured configuration rather than `sed`-modifying shell scripts.
9. The official startup path does not require manually opening three terminals.
10. `awin doctor` or equivalent verifies GPU, models, service ports, WebRTC, microphone context, and critical dependencies.
11. Awin remains available in text-only degraded mode.
12. TTS/avatar playback supports interruption and cancellation.
13. Streaming-first speech output is the target architecture.

---

## 16. Recommended Next OpenSpec Work

When formalizing Change 003, the design should add these sub-workstreams:

```text
003-A provider contracts
003-B model registry
003-C bootstrap / doctor
003-D speech runtime
003-E avatar worker
003-F presence gateway
003-G executive cockpit
003-H private asset policy
003-I performance benchmark profiles
```

Benchmark at least:

- first transcript latency;
- LLM time-to-first-token;
- TTS time-to-first-audio;
- avatar infer FPS;
- final output FPS;
- end-to-end first-response latency;
- VRAM usage by service;
- interruption cancellation latency.

---

## 17. Final Recommendation

Use the Freedidi tutorial as a useful integration case study, especially for practical WSL, VAD, VRAM, WebRTC, and avatar-preparation lessons.

Do **not** make its one-click package a dependency.

The Awin implementation should become a reproducible first-party stack:

> **Official sources + pinned models + automated bootstrap + provider-neutral runtime + private assets + no Quark dependency.**

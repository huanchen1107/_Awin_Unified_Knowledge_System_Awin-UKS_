# Awin CEO Executive Presence / Digital Human — Architecture Discussion

**Date:** 2026-09-05  
**Status:** Planning discussion  
**Planning source:** Chairman + ChatGPT  
**OpenSpec status:** Not created  
**Change ID:** TBD by Antigravity after repository inspection  

## 1. Purpose

Design a customizable digital-human interface for **Awin**, the current CEO identity in the AI Company, without confusing presentation technology with organizational identity or CEO reasoning.

Canonical invariant:

```text
Position != Role != Identity != Agent != Runtime != System != Presentation
```

For this topic:

```text
CEO position
   ↓ current binding
Awin identity / CEO agent
   ↓ presentation binding
Awin Executive Presence
   ├─ avatar
   ├─ voice
   ├─ speech I/O
   └─ executive cockpit
```

Awin remains fully usable without the digital-human layer.

## 2. Relationship to Existing Roadmap

Do NOT assign this planning topic to Change 003. Canonical roadmap already uses:

```text
003 — Agent Runtime & Execution Fabric
005 — Awin CEO Orchestrator
006 — AI Company Control Center
```

Avatar / Executive Presence potentially spans 005 and 006:

```text
005 Awin CEO Orchestrator
  owns CEO intent, delegation, synthesis, executive behavior

006 AI Company Control Center
  owns human-facing company operating surface

Executive Presence
  presents Awin through voice/avatar and maps structured CEO events to the Control Center UI
```

Antigravity must inspect the current repository and decide whether this becomes:

1. planning sources added to 005/006;
2. a sub-capability implemented across 005/006;
3. a future standalone Change with a new ID.

ChatGPT planning must not pre-allocate that Change ID.

## 3. Target Experience

The target is not a decorative talking face. It is an **Executive Presence Layer** integrated with an **Executive Cockpit**.

```text
Chairman
   │ voice / text
   ▼
Executive Cockpit
   │
   ├─ Digital Human Awin
   ├─ company health
   ├─ active missions/projects
   ├─ decisions requiring Chairman
   ├─ CxO/agent status
   ├─ UKS evidence
   └─ delegation/actions
   │
   ▼
Presence Gateway
   │
   ▼
Awin CEO Orchestrator
```

The visual interface should expose real states rather than fake animation:

```text
IDLE
LISTENING
THINKING
RETRIEVING_KNOWLEDGE
DELEGATING
WAITING_FOR_APPROVAL
EXECUTING
SPEAKING
DEGRADED
ERROR
```

## 4. Technical Reference Case

The Freedidi local digital-human tutorial is treated as an integration case study, not a package dependency. It combines:

- llama.cpp + local Qwen GGUF
- VAD
- faster-whisper
- Qwen3-TTS voice cloning
- LiveTalking
- Wav2Lip
- WebRTC
- ComfyUI + Wan2.2 for idle avatar video preparation

Important practical findings preserved from that case:

- local deployment can fit on a 24 GB class GPU when using a quantized LLM and Wav2Lip;
- WSL2 networking and browser WebRTC/microphone behavior need diagnostics;
- reference audio transcript accuracy matters for zero-shot voice cloning;
- VAD silence thresholds materially affect interruption and premature turn-taking;
- ComfyUI/Wan2.2 are asset creation tools, not required runtime services;
- three-terminal manual startup is suitable for a tutorial, not a production architecture.

## 5. No-Quark / Reproducibility Policy

Official Awin deployment must not require Quark Cloud or opaque one-click archives.

Preferred source priority:

```yaml
artifact_sources:
  - official_github_repository_or_release
  - huggingface
  - modelscope
  - official_download_script
  - upstream_documented_google_drive
```

Every required model should have reproducible metadata where practical:

```yaml
model:
  id: example
  source: official_or_approved
  revision: pinned
  checksum: optional_sha256
  license: recorded
  local_path: models/...
```

Private biometric media must remain outside public Git history.

## 6. Runtime Architecture

```text
Chairman microphone / text
        │
        ▼
Input Adapter
        │
        ├─ VAD
        └─ STT Provider
             default candidate: faster-whisper
        │
        ▼
Presence Gateway
        │
        ▼
Awin CEO Orchestrator
        │
        ├─ UKS / CIO knowledge retrieval
        ├─ Mission / delegation system
        └─ LLM runtime/provider
        │
        ▼
Structured Executive Response
        │
        ├─ speech_text
        ├─ UI cards
        ├─ actions
        ├─ citations/evidence refs
        ├─ priority
        └─ state
        │
        ├──────────────► Executive Cockpit
        │
        ▼
TTS Provider
        │
        ▼
Avatar Provider
        │
        ▼
WebRTC / media stream
```

Awin Core should never depend directly on LiveTalking, Wav2Lip, MuseTalk or Qwen3-TTS.

## 7. Provider Contracts

### STT Provider

```text
open_stream()
transcribe_chunk()
finalize_utterance()
cancel()
health()
```

### TTS Provider

```text
open_stream(voice_profile)
push_text_chunk()
stream_audio()
cancel()
health()
```

### Avatar Provider

```text
create_session(profile)
push_audio_chunk()
set_state()
interrupt()
close_session()
health()
```

### Presence Gateway

```text
accept_input()
invoke_awin()
stream_structured_response()
publish_ui_events()
route_audio()
propagate_interrupt()
```

## 8. Recommended Media Profiles

### 24 GB single-GPU profile — default candidate

```yaml
llm: quantized_local_qwen_or_equivalent
stt: faster-whisper
stt_size: adaptive
tts: qwen3_tts
avatar_framework: livetalking
lip_sync: wav2lip
```

Reason: the Freedidi reference shows Wav2Lip can keep avatar VRAM near roughly 1–2 GB while LLM, STT and TTS share the same GPU.

### High-quality profile

```yaml
avatar_framework: livetalking
lip_sync: musetalk_1_5
```

Use when dedicated GPU or sufficient VRAM headroom exists.

### Hybrid profile

Allow remote LLM with local STT/TTS/avatar, or other provider substitutions permitted by company policy.

## 9. Avatar Asset Preparation

Runtime must be separated from avatar creation.

```text
Awin portrait
   ↓
ComfyUI / equivalent
   ↓
Wan2.2 I2V / equivalent
   ↓
short closed-mouth idle video
   ↓
Avatar Provider preprocessing
   ↓
awin_ceo_v1
```

ComfyUI and Wan2.2 should not be required to start Awin after the avatar asset has been prepared.

## 10. Voice Profile

```yaml
presentation_bindings:
  awin:
    avatar_profile: awin_ceo_v1
    voice_profile: awin_voice_v1
    presence_profile: executive_ceo

voice_profiles:
  awin_voice_v1:
    provider: qwen3_tts
    mode: zero_shot_clone
    reference_audio: private-assets/voices/awin/ref.wav
    reference_text: private-assets/voices/awin/ref.txt
    locale: zh-TW
```

Rules:

- voice/avatar profiles are versioned;
- presentation binding may change without changing CEO identity;
- private raw audio/video stays gitignored/private;
- provider may be replaced without changing Awin business contracts.

## 11. Streaming-First + Barge-In

Do not wait for a complete response before speaking.

```text
LLM stream
   ↓
phrase/sentence chunker
   ↓
streaming TTS
   ↓
audio chunks
   ↓
avatar
```

Chairman interruption must propagate end-to-end:

```text
new human speech
   ↓
VAD detects barge-in
   ↓
cancel pending CEO speech chunks
   ↓
stop TTS
   ↓
stop avatar playback
   ↓
mark previous output interrupted
   ↓
accept next utterance
```

This is a protocol requirement, not merely a front-end behavior.

## 12. Executive Response Contract

Awin should return more than prose:

```yaml
executive_response:
  speech: "董事長，目前有兩件事情需要您決定。"
  state: WAITING_FOR_APPROVAL
  priority: high
  cards:
    - type: decision_required
      mission_id: M-...
      title: Review deployment profile
  evidence_refs:
    - UKS:...
  suggested_actions:
    - approve
    - request_revision
    - open_evidence
```

The avatar speaks `speech`; the Control Center renders cards/actions/evidence.

## 13. Deployment and Service Boundary

Recommended logical services:

```text
awin-ceo-orchestrator
uks
runtime-fabric
speech-runtime
avatar-worker
presence-gateway
control-center
```

The exact process/container split must be decided by Antigravity after inspecting repository conventions.

Target operator experience may eventually become:

```text
bootstrap
awin doctor
awin start
```

or an equivalent managed compose/supervisor flow.

`doctor` should validate GPU/VRAM, model availability, ports, WebRTC/ICE, microphone secure context, WSL conditions where relevant, service health and private asset configuration.

## 14. Degraded Modes

```text
Avatar failure → voice + text UI
TTS failure    → text UI
STT failure    → typed input
local LLM fail → permitted fallback runtime/provider
UKS failure    → state evidence retrieval unavailable explicitly
```

Awin CEO must never become unavailable solely because the avatar service fails.

## 15. Security / Privacy

- treat voice-clone and avatar source media as sensitive private assets;
- never commit private media into public Git history;
- log actions/events, not unnecessary raw microphone streams;
- preserve Chairman approval gates;
- expose provenance for UKS-derived statements;
- do not let media-layer plugins gain implicit business authority;
- keep external model/plugin licenses recorded.

## 16. Antigravity Handoff

Antigravity must not start by creating a new OpenSpec Change from this document alone.

Required sequence:

```text
1. Read AGENTS.md
2. Read _myplan_/planning-manifest.yaml
3. Read _myplan_/change-registry.yaml
4. Read roadmap + 005/006 planning sources
5. Read this discussion + accepted decision
6. Inspect current source tree and existing OpenSpec artifacts
7. Resolve architectural ownership
8. Decide whether to update existing planned Change(s) or propose a new Change ID
9. Record planning provenance
10. Only then enter the OpenSpec lifecycle
```

If implementation or repo reality conflicts with this plan, write a feedback record under `_myplan_/feedback/`; do not silently rewrite accepted planning.

## 17. Evaluation Questions for Antigravity

Before formalization, answer:

- Is Presence primarily part of 006 Control Center, or does runtime/media complexity justify a separate future Change?
- What existing runtime abstractions from 003 can Presence reuse?
- Which CEO contracts from 005 should expose executive response/state events?
- Can LiveTalking remain isolated behind a clean provider boundary?
- What measured VRAM/latency profile is realistic on the target hardware?
- Which upstream versions/licenses should be pinned?
- What is the minimal MVP that proves text → speech → lip-sync → interrupt → degraded mode?

## 18. Desired Result

The product target is:

> **Awin = CEO intelligence and executive behavior**  
> **Executive Presence = Awin's replaceable visual/voice presentation**  
> **Control Center = company operating interface**  
> **UKS = enterprise knowledge system**

The digital human should make the AI Company easier to operate, not make the company's architecture depend on a face renderer.

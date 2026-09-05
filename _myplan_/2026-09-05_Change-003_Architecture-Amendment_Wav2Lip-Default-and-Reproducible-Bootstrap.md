# Change 003 Architecture Amendment — Wav2Lip Default + Reproducible Bootstrap

**Date:** 2026-09-05  
**Applies to:** `003-awin-ceo-digital-human-executive-interface`

## Amendment Summary

This amendment updates the initial Change 003 planning based on the more complete Freedidi deployment details and current open-source alternatives.

### Revised default avatar profile

For a single 24 GB GPU workstation running LLM + STT + TTS + avatar on the same GPU:

```yaml
avatar_engine_profiles:
  single_gpu_default:
    provider: livetalking
    lip_sync: wav2lip
    rationale: lower_vram_and_realtime_headroom

  high_quality:
    provider: livetalking
    lip_sync: musetalk_1_5
    rationale: better_visual_quality_when_gpu_headroom_exists
```

Wav2Lip is therefore no longer treated only as a fallback. It becomes the Phase 1 default for the practical single-GPU profile.

## Bootstrap Policy

The official Awin implementation must be reproducible from GitHub plus approved upstream model/software sources.

```yaml
bootstrap_policy:
  quark_cloud_required: false
  opaque_bundle_required: false
  reproducible: true
  pinned_versions: true
  checksum_verification: preferred
```

Target experience:

```bash
git clone <awin-repository>
cd <awin-repository>
./scripts/bootstrap.sh
awin doctor
awin start
```

or equivalent managed container workflow.

## Runtime vs Setup-Time Tools

Runtime:

```text
Awin Core
Presence Gateway
LLM Worker
Speech Runtime
LiveTalking Avatar Worker
Executive Cockpit
UKS integration
```

Setup-time only:

```text
ComfyUI
Wan2.2
avatar image/video preparation
```

ComfyUI/Wan2.2 must not become required dependencies for everyday Awin startup.

## Replace Freedidi Glue Scripts

Do not depend on:

```text
install-voice.sh
start-voice.sh
start-livetalking.sh
avatar-sync.js
```

from a third-party bundle.

Reimplement their responsibilities as first-party Awin components:

```text
scripts/bootstrap/
services/speech-runtime/
services/avatar-worker/
services/presence-gateway/
```

## Streaming-First Requirement

The architecture should evolve from whole-response synthesis toward:

```text
LLM token stream
→ phrase chunker
→ streaming TTS
→ audio chunks
→ avatar renderer
→ WebRTC
```

Chairman interruption must cancel pending LLM/TTS/avatar output quickly and return to listening state.

## New Acceptance Criteria

1. No mandatory Quark Cloud dependency.
2. No opaque one-click package dependency.
3. Official/traceable source recorded for every required model.
4. Model revision/version can be pinned.
5. Single-GPU default uses Wav2Lip unless benchmark evidence supports a different choice.
6. MuseTalk 1.5 remains an optional high-quality profile.
7. Private voice/avatar training/reference media is excluded from public Git.
8. `awin doctor` or equivalent validates GPU, ports, models, WebRTC, microphone access, and service health.
9. Startup does not require manually maintaining three terminal windows.
10. Text-only degraded mode remains available if digital-human services fail.
11. ComfyUI/Wan2.2 are avatar-creation tools only.
12. Streaming and interruption are first-class architectural requirements.

## Related Research

See:

`_myplan_/2026-09-05_Change-003_No-Quark-Reproducible-Digital-Human-Stack-Research.md`

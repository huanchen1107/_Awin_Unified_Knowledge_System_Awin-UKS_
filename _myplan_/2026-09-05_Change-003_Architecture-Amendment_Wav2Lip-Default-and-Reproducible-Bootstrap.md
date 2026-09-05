# HISTORICAL AMENDMENT — Wav2Lip Default + Reproducible Bootstrap

**Date:** 2026-09-05  
**Status:** Historical planning input; superseded as a Change-003 amendment  
**Important:** Canonical Change 003 is `agent-runtime-execution-fabric`.

Canonical current Avatar / Executive Presence planning:

- `_myplan_/discussions/2026-09-05_Awin-CEO-Executive-Presence-Digital-Human_Architecture-Discussion.md`
- `_myplan_/decisions/2026-09-05_Awin-CEO-Executive-Presence_Planning-Boundary-and-Stack.md`

This file preserves the technical amendment that remains useful for Antigravity evaluation.

## Technical Amendment Summary

For a single 24 GB GPU workstation running LLM + STT + TTS + avatar on the same GPU, use the following as the first benchmark candidate:

```yaml
avatar_engine_profiles:
  single_gpu_default_candidate:
    provider: livetalking
    lip_sync: wav2lip
    rationale: lower_vram_and_realtime_headroom

  high_quality_candidate:
    provider: livetalking
    lip_sync: musetalk_1_5
    rationale: better_visual_quality_when_gpu_headroom_exists
```

Wav2Lip is therefore not merely a fallback candidate. It should be benchmarked as the practical first profile for a 24 GB single-GPU target.

## Bootstrap Policy

Official Awin implementation should be reproducible from GitHub plus approved upstream model/software sources.

```yaml
bootstrap_policy:
  quark_cloud_required: false
  opaque_bundle_required: false
  reproducible: true
  pinned_versions: true
  checksum_verification: preferred
```

Target operator experience may eventually be:

```bash
git clone <awin-repository>
cd <awin-repository>
./scripts/bootstrap.sh
awin doctor
awin start
```

or equivalent managed container workflow.

## Runtime vs Setup-Time Tools

Runtime candidates:

```text
Awin CEO Orchestrator
Presence Gateway
Runtime Fabric / LLM Worker
Speech Runtime
LiveTalking Avatar Worker
AI Company Control Center
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

Do not make third-party package scripts architectural dependencies:

```text
install-voice.sh
start-voice.sh
start-livetalking.sh
avatar-sync.js
```

Reimplement the required responsibilities as first-party, provider-neutral components after Antigravity resolves ownership and repository placement.

## Streaming-First Requirement

```text
LLM token stream
→ phrase chunker
→ streaming TTS
→ audio chunks
→ avatar renderer
→ WebRTC
```

Chairman interruption must cancel pending speech/avatar output and return the session to listening state quickly.

## Evaluation Criteria for Antigravity

1. No mandatory Quark Cloud dependency.
2. No opaque one-click package dependency.
3. Official/traceable source recorded for required models.
4. Model revisions can be pinned.
5. Benchmark Wav2Lip as the 24 GB single-GPU profile.
6. Benchmark MuseTalk 1.5 as the high-quality profile.
7. Private voice/avatar reference media is excluded from public Git.
8. Provide `doctor`-style checks for GPU, ports, models, WebRTC, microphone access and service health.
9. Avoid a permanent three-terminal startup model.
10. Preserve text-only degraded mode.
11. Keep ComfyUI/Wan2.2 as asset-creation tools.
12. Treat streaming and interruption as protocol requirements.

## Governance Rule

This amendment is **planning evidence**, not permission to create an OpenSpec Change. Antigravity must first inspect `AGENTS.md`, `_myplan_/planning-manifest.yaml`, `_myplan_/change-registry.yaml`, related Changes 003/005/006, current OpenSpec artifacts and the source tree, then decide ownership and any future Change ID.

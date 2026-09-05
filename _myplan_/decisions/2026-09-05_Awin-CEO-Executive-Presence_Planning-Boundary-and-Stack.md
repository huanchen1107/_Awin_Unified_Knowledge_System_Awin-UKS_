# Decision — Awin CEO Executive Presence Planning Boundary and Stack

**Date:** 2026-09-05  
**Status:** Accepted planning decision  
**Decision owner:** Chairman  
**Planning source:** Chairman + ChatGPT  

## Decision

Awin's digital-human capability is defined as **Executive Presence**, a replaceable presentation layer for the CEO identity.

It is **not** assigned to OpenSpec Change 003. Change numbering for this capability remains **TBD by Antigravity** after repository inspection.

## Canonical boundary

```text
CEO position
  ↓
Awin identity / CEO agent
  ↓
Executive Presence presentation
  ├─ avatar
  ├─ voice
  ├─ speech input/output
  └─ cockpit presentation
```

The following must remain separate:

```text
Organization role != identity != agent != runtime != system != presentation
Awin CEO Core != Avatar Provider
UKS != Awin identity
Control Center != CEO reasoning
```

## Roadmap relationship

Current canonical roadmap already contains:

```text
003 Agent Runtime & Execution Fabric
005 Awin CEO Orchestrator
006 AI Company Control Center
```

Executive Presence is a planning topic that may consume contracts from 003, expose CEO behavior from 005, and render within or alongside 006.

Antigravity must resolve final OpenSpec ownership.

## Technology direction

Preferred initial stack candidates:

```text
STT: faster-whisper
TTS: Qwen3-TTS
Avatar framework: LiveTalking
Lip sync default for 24GB single GPU: Wav2Lip
High-quality profile: MuseTalk 1.5
Transport: WebRTC
Asset creation only: ComfyUI + Wan2.2
```

These are implementation candidates, not domain dependencies.

## Reproducibility policy

Official Awin setup must not require Quark Cloud / 夸克網盤 or opaque third-party one-click bundles.

Use official/traceable sources, pinned versions, model registry metadata and private asset separation.

## Runtime requirements

- streaming-first speech output;
- barge-in/interruption propagated through TTS and avatar playback;
- text-only degraded mode;
- structured executive responses for cockpit cards/actions/evidence;
- provider-neutral STT/TTS/avatar contracts;
- media subsystem failure must not take down Awin CEO.

## Governance workflow

```text
ChatGPT / Chairman planning
→ _myplan_
→ manifest / registry mapping
→ Antigravity repository inspection
→ Antigravity chooses OpenSpec ownership/change ID
→ OpenSpec lifecycle
→ implementation/test evidence
→ _myplan_/feedback
```

This decision must be cited as planning provenance if Antigravity later formalizes Executive Presence in OpenSpec.

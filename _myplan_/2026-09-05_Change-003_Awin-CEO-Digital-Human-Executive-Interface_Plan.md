# SUPERSEDED — Awin CEO Digital Human Executive Interface Planning Note

**Date:** 2026-09-05  
**Status:** Superseded as a Change-numbered document  
**Reason:** `003` is canonically reserved for `agent-runtime-execution-fabric` in the AI Company roadmap.  
**Do not create OpenSpec from this file directly.**

This file is retained only as planning history.

The canonical Avatar / Executive Presence planning has moved to:

- `_myplan_/discussions/2026-09-05_Awin-CEO-Executive-Presence-Digital-Human_Architecture-Discussion.md`
- `_myplan_/decisions/2026-09-05_Awin-CEO-Executive-Presence_Planning-Boundary-and-Stack.md`

## Correct AI Company workflow

```text
Chairman / ChatGPT planning
        ↓
_myplan_/ discussions + decisions + roadmap
        ↓
planning-manifest.yaml
        ↓
change-registry.yaml
        ↓
Antigravity reads repository + planning provenance
        ↓
Antigravity decides whether work belongs to an existing Change
or requires a new Change
        ↓
OpenSpec lifecycle
        ↓
implementation / tests / evidence
        ↓
_myplan_/feedback/
        ↓
planning review
```

## Preserved architectural intent

The original architectural idea remains valid:

- Awin is the CEO agent, not the avatar.
- Avatar is an Executive Presence / Digital Human interface.
- UKS remains a knowledge system managed by the CIO position.
- Role, identity, agent, runtime, voice, avatar and UI presentation must remain independently bindable.
- Avatar/STT/TTS engines must be provider-replaceable.
- Awin must retain text-only degraded operation if media services fail.

All detailed architecture and current technology recommendations are maintained in the canonical discussion/decision files listed above.

# Change 003 — Awin CEO Digital Human Executive Interface

**Date:** 2026-09-05  
**Planning Source:** ChatGPT project discussion  
**Repository:** `_Awin_Unified_Knowledge_System_Awin-UKS_`  
**Status:** Planning / not yet implementation  
**Proposed OpenSpec Change ID:** `003-awin-ceo-digital-human-executive-interface`

---

## 1. Goal

建立 AI Company 的 CEO Agent — **Awin** — 的數字人互動介面。

核心原則：

> **Awin ≠ Avatar.**  
> Awin 是 CEO Agent；Avatar 是 Awin 的 Executive Presence / Human Interface。

數字人不得成為 CEO reasoning、memory、governance 或 UKS 的核心依賴。Avatar engine、TTS、STT 都必須可以替換。

---

## 2. Organization Boundary

延續 Change 001 / 002 的責任分離：

- **Chairman**：人類最高決策者。
- **CEO / Awin**：公司級 AI orchestration、規劃、委派、彙整、決策支援。
- **CIO / UKS**：Unified Knowledge System；負責企業知識檢索、來源整合與 evidence retrieval。
- **Avatar / Presence Layer**：Awin 的臉、聲音、口型、輸入輸出與 Executive Cockpit；不是 UKS。
- 未來 CTO / CFO / COO / CISO 等職位可依相同 role-binding 模型加入。

因此 Change 003 不應把數字人功能塞入 UKS 核心。

---

## 3. Target Architecture

```text
                         Chairman
                            │
                    Voice / Text / UI
                            │
                            ▼
              ┌─────────────────────────┐
              │ Awin Executive Presence │
              │ Digital Human Interface │
              └────────────┬────────────┘
                           │
                  Presence Gateway
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
       STT                TTS             Avatar Engine
   Whisper/etc.       pluggable TTS       LiveTalking/etc.
                                               │
                                               ▼
                                       Lip Sync / Video
                           │
                           ▼
                 ┌──────────────────┐
                 │  Awin CEO Core   │
                 │ Agent / Planner  │
                 │ Governance       │
                 │ Delegation       │
                 └────────┬─────────┘
                          │
            ┌─────────────┼─────────────┐
            │             │             │
            ▼             ▼             ▼
          UKS CIO      GitHub /       Other CxO
       Knowledge       OpenSpec       Agents
       Retrieval       Workflows
```

---

## 4. Reference Direction

Initial research references supplied during planning:

- `https://www.freedidi.com/24928.html`
- `https://www.freedidi.com/24984.html`

Primary implementation direction to investigate:

- LiveTalking-style real-time digital human pipeline
- customizable avatar identity
- lip synchronization
- replaceable STT / LLM / TTS / avatar components
- browser-based interactive interface
- local/self-hosted capability where practical

Reference implementations are architectural inspiration only. Do not couple Awin business logic to any single upstream project.

---

## 5. Awin Executive Presence Layer

Introduce an explicit abstraction:

```text
Awin CEO Core
     │
     ▼
Presence Gateway
     │
     ├── Input Adapter
     │    ├── text
     │    └── speech/STT
     │
     ├── Response Stream
     │
     ├── Voice Adapter
     │    └── TTS / voice profile
     │
     ├── Avatar Adapter
     │    └── animation / lip-sync / video stream
     │
     └── UI Event Adapter
          └── cards / decisions / projects / delegation
```

Awin Core should return structured responses rather than only text:

```yaml
response:
  speech: "董事長，目前有三件事情需要注意。"
  emotion: professional
  priority: high
  cards:
    - type: decision_required
      project: AwinFinTech
      title: Review Change 030
  actions:
    - open_project
    - delegate
    - view_executive_brief
```

The avatar speaks `speech`; Executive Cockpit renders structured data and actions.

---

## 6. Executive Cockpit UI

Do not design this as a full-screen talking face only.

Recommended desktop layout:

```text
┌─────────────────────────────────────────────────────────┐
│ AI COMPANY                                  Chairman     │
├──────────────────────────────┬──────────────────────────┤
│                              │ AWIN                     │
│                              │ Chief Executive Officer  │
│       Digital Human          │ ● ACTIVE                 │
│          Awin                │                          │
│                              │ Company Health           │
│                              │ Active Projects          │
│                              │ Decisions Required       │
│                              │ Agent Status             │
│                              │                          │
│                              │ Executive Brief          │
│                              │ 1. ...                   │
│                              │ 2. ...                   │
├──────────────────────────────┴──────────────────────────┤
│ 🎙 Ask Awin...                              [Send]       │
├─────────────────────────────────────────────────────────┤
│ Brief | Projects | Decisions | Agents | UKS | Delegate │
└─────────────────────────────────────────────────────────┘
```

The interface should support:

1. conversational CEO interaction;
2. current Awin activity/state;
3. company/project health;
4. decisions requiring Chairman approval;
5. active agent/CxO status;
6. executive brief;
7. delegation actions;
8. UKS evidence/query access;
9. OpenSpec/GitHub work status;
10. voice and text fallback.

---

## 7. Role / Agent / Avatar Binding

Preserve the role-binding model established in Change 001.

Example:

```yaml
organization:
  chairman:
    role_id: CHAIRMAN
    binding_type: human

  ceo:
    role_id: CEO
    agent_binding: awin

  cio:
    role_id: CIO
    system_binding: UKS

presentation_bindings:
  awin:
    avatar_profile: awin_ceo_v1
    voice_profile: awin_voice_v1
    presence_profile: executive_ceo
```

This deliberately separates:

```text
ROLE → AGENT → PRESENTATION
```

Changing Awin's face must not change the CEO role.
Changing the CEO agent must not change organization policy.
Changing LiveTalking must not change Awin Core.

---

## 8. Provider-Neutral Contracts

Define interfaces before choosing permanent engines.

### AvatarProvider

```text
create_session()
set_avatar(profile)
push_audio(stream)
set_expression(state)
interrupt()
close_session()
```

### SpeechToTextProvider

```text
transcribe(audio_stream)
```

### TextToSpeechProvider

```text
synthesize(text, voice_profile)
stream(text, voice_profile)
interrupt()
```

### PresenceGateway

```text
accept_user_input()
invoke_awin()
stream_response()
publish_ui_events()
interrupt_response()
```

LiveTalking can be the first AvatarProvider implementation without becoming a domain dependency.

---

## 9. Interaction State Machine

A digital CEO needs visible state, not just lip-sync.

```text
IDLE
  ↓
LISTENING
  ↓
THINKING
  ↓
SPEAKING
  ↓
IDLE
```

Additional states:

- RETRIEVING_KNOWLEDGE
- DELEGATING
- WAITING_FOR_APPROVAL
- EXECUTING
- ERROR / DEGRADED

UI and avatar should reflect these states consistently.

Example:

```text
Awin · THINKING
UKS · Retrieving 4 sources...
```

rather than pretending Awin already knows an answer while retrieval is still running.

---

## 10. Interruptibility

A CEO interface must support natural interruption.

If Chairman speaks while Awin is talking:

```text
Chairman interruption
        ↓
stop TTS
        ↓
stop avatar audio/lip-sync
        ↓
cancel/mark current response
        ↓
accept new utterance
        ↓
Awin Core
```

This should be a first-class contract rather than a UI hack.

---

## 11. Privacy / Security

Voice cloning and custom avatars may contain biometric/personal media. Requirements:

- avatar/voice assets are configurable and access-controlled;
- do not commit private raw voice/video training assets to public GitHub;
- configuration should reference asset IDs/paths, not embed sensitive media;
- clearly separate local/private assets from source code;
- log agent actions, but avoid unnecessarily logging raw microphone streams;
- Chairman approval gates remain authoritative for sensitive actions.

---

## 12. Deployment Strategy

Do not require the public web application to run GPU-heavy avatar generation directly.

Recommended split:

```text
Web / Executive Cockpit
        │
        ▼
Awin API / Presence Gateway
        │
        ├── CEO Agent services
        ├── UKS
        └── Digital Human Worker
              ├── STT
              ├── TTS
              └── Avatar / GPU
```

Possible deployment modes:

### Mode A — Local Workstation
Best initial prototype. GPU worker runs locally.

### Mode B — Hybrid
Cockpit/API in cloud; private avatar worker on local GPU machine.

### Mode C — Full Server
GPU-enabled server runs the presence pipeline.

The architecture must support all three without changing Awin's domain contracts.

---

## 13. Implementation Phases

### Phase 0 — Spike

- evaluate LiveTalking and alternatives;
- confirm browser streaming mechanism;
- measure first-audio and lip-sync latency;
- test Mandarin Chinese;
- test interruption;
- test custom avatar workflow;
- document GPU requirements.

### Phase 1 — Awin Avatar MVP

Deliver:

- one customizable Awin CEO avatar;
- one voice profile;
- text input;
- speech output;
- lip-sync;
- browser UI;
- provider abstraction.

No deep UKS integration is required to prove the presence pipeline.

### Phase 2 — Real-Time Conversation

Add:

- microphone input;
- STT;
- streaming response;
- streaming TTS;
- interruption/barge-in;
- LISTENING / THINKING / SPEAKING states.

### Phase 3 — Executive Cockpit

Add:

- company health;
- project cards;
- decision-required queue;
- agent/CxO status;
- executive brief;
- delegation UI.

### Phase 4 — UKS / Company Integration

Awin can request CIO/UKS evidence and present:

```text
Question → Awin CEO → UKS CIO → evidence → Awin synthesis → Chairman
```

The UI should expose source/evidence provenance separately from the avatar's spoken summary.

### Phase 5 — Multi-Agent Executive Team

Extend presentation bindings to CTO/CFO/COO/CISO agents if needed. Do not duplicate the entire presence architecture per agent.

---

## 14. Suggested Repository Structure

```text
apps/
  executive-cockpit/

services/
  awin-core/
  presence-gateway/
  digital-human-worker/

packages/
  organization-contracts/
  presence-contracts/
  ui-events/

config/
  organization.yaml
  agent-bindings.yaml
  presentation-bindings.yaml

private-assets/          # gitignored
  avatars/
  voices/

openspec/
  changes/
    003-awin-ceo-digital-human-executive-interface/
```

Exact paths should be adapted to the repository's actual structure during OpenSpec proposal creation rather than imposed blindly.

---

## 15. Non-Goals for Change 003

Do **not**:

- redesign UKS retrieval internals;
- merge CIO and CEO responsibilities;
- bind CEO identity permanently to a face;
- hard-code LiveTalking into Awin Core;
- put private voice/video assets in public Git history;
- implement every future CxO avatar;
- make visual realism more important than low latency and reliable interaction.

---

## 16. Acceptance Criteria

Change 003 can eventually be considered successful when:

1. Chairman can open Executive Cockpit and interact with Awin by text and voice.
2. Awin has a customizable CEO avatar and voice.
3. Speech and lip movement are synchronized adequately for interactive use.
4. Awin can be interrupted while speaking.
5. UI exposes real execution states instead of fake activity.
6. Avatar/TTS/STT implementations can be replaced behind stable interfaces.
7. Awin Core remains independent of LiveTalking or any specific renderer.
8. UKS remains a CIO/knowledge capability, not the avatar engine.
9. Role/agent/presentation bindings remain independent.
10. Executive Cockpit can display structured company/project/decision information in addition to conversation.
11. Private biometric media is excluded from public source control.
12. Degraded mode permits text-only Awin operation when digital-human services are unavailable.

---

## 17. Recommended OpenSpec Handoff

When implementation begins, ask Antigravity to create the formal OpenSpec change:

```text
003-awin-ceo-digital-human-executive-interface
```

It should inspect Change 001 and Change 002 first and produce at minimum:

```text
proposal.md
design.md
tasks.md
specs/
```

The formal design must preserve these invariants:

```text
Organization Role != Agent Identity != Avatar Identity
Awin CEO Core != Presence Layer
Awin CEO != UKS CIO
Domain Logic != Digital Human Provider
```

---

## 18. Architectural Decision

**Recommended direction:** proceed with Change 003 as an independent **Awin Executive Presence / Digital Human Layer**.

Use LiveTalking-style technology as the first implementation candidate, but design provider-neutral contracts from day one.

The desired product is not merely a talking avatar. It is a visual and conversational **CEO operating surface for the AI Company**:

> **Awin = CEO intelligence**  
> **Avatar = executive presence**  
> **Executive Cockpit = company operating interface**  
> **UKS = CIO knowledge intelligence**

# Architecture Decision — CIO Identity = Pick; UKS Reserved for the System

**Date:** 2026-09-05  
**Status:** Accepted / Canonical

## Decision

The canonical naming is:

```text
Chairman / 董事長 → Owner / User
CEO / 執行長      → Awin
CIO / 資訊長      → Pick
UKS               → Unified Knowledge System
```

`Pick` is the CIO identity. `UKS` is a technical System.

## Canonical relationship

```text
Chairman
   ↓
CEO — Awin
   ↓
CIO — Pick
   │ manages
   ▼
UKS — Unified Knowledge System
```

The company uses binding semantics:

```text
Position ── Binding ──> Identity
```

Initial bindings:

```yaml
bindings:
  chairman: owner
  ceo: awin
  cio: pick
```

These bindings may change later without redefining the positions or workflow.

## Domain invariant

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

Therefore:

```text
CIO      = Position
Pick     = Identity
CIO Agent= Agent
Claude   = possible Runtime
UKS      = System
```

No model/provider should be hard-coded as Pick's identity.

## Superseded naming

Any older planning text containing these patterns is historical and superseded:

```text
CIO — UKS
UKS = CIO Identity
Identity: UKS
UKP required only because UKS name is occupied by the CIO
```

The corrected interpretation is always:

```text
CIO — Pick
UKS — Unified Knowledge System
```

## Roadmap consequence

The naming correction also supports the AI Company-first architecture:

```text
001 AI Company Organization Foundation
002 Mission & Company Orchestration
003 Agent Runtime & Execution Fabric
004 UKS — Unified Knowledge System
005 Awin CEO Orchestrator
006 AI Company Control Center
```

UKS is intentionally implemented later as a company system managed under CIO scope.

## Implementation rule

Future code/config/specs should use stable IDs such as:

```text
position_id: cio
identity_id: pick
system_id: uks
```

Do not reuse `uks` as an identity identifier.

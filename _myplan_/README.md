# `_myplan_` — Canonical Planning Index

This folder contains planning material for the Awin AI Company project.

## Canonical organization

```text
Chairman / 董事長 → Owner / User
CEO / 執行長      → Awin
CIO / 資訊長      → Pick
UKS               → Unified Knowledge System
```

Invariant:

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

## Canonical roadmap

```text
001 — AI Company Organization Foundation
002 — Mission & Company Orchestration
003 — Agent Runtime & Execution Fabric
004 — UKS: Unified Knowledge System
005 — Awin CEO Orchestrator
006 — AI Company Control Center
```

## What to read first

1. `roadmap/2026-09-05_AI-Company-Orchestration_Integrated-Plan.md` — current integrated planning baseline.
2. `2026-09-05_Awin-UKS_Organizational-Governance-and-Role-Binding_Plan.md` — Change 001 organization/governance planning input.
3. `2026-09-05_Change-001-002_Scope-Separation.md` — current scope boundary and 001–006 allocation.
4. `decisions/` — accepted architecture decisions.
5. `discussions/` — design rationale and historical discussion.

## Document semantics

### `roadmap/`
Current integrated direction and sequence of changes.

### `decisions/`
Accepted architecture decisions. These supersede conflicting historical discussion.

### `discussions/`
Reasoning, alternatives, and historical context. Useful for understanding *why*, but not the highest-priority specification source.

## Precedence

```text
Accepted OpenSpec / canonical specs
        ↓
Canonical design contracts
        ↓
Current implementation + tests
        ↓
Accepted architecture decisions
        ↓
Integrated roadmap
        ↓
Planning discussions / historical notes
```

## Important superseded concepts

Do not use these older interpretations:

```text
CIO = UKS identity
Change 002 = UKS System Foundation
Awin = merely a knowledge-search butler/router
```

Use instead:

```text
CIO = Position
Pick = current CIO identity
UKS = Unified Knowledge System
Awin = current CEO identity
Mission = durable company work unit
Change 002 = Mission & Company Orchestration
Change 004 = UKS
```

## Current next step

The immediate planning target is:

```text
Change 001 — AI Company Organization Foundation
```

After Change 001 contracts are stable, proceed to:

```text
Change 002 — Mission & Company Orchestration
```

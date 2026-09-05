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

## Machine-readable planning bridge

Agents MUST NOT guess which planning documents apply to a Change.

Read these first:

1. `planning-manifest.yaml` — canonical organization, invariants, roadmap, accepted decisions, and Change→source mapping.
2. `change-registry.yaml` — scope, non-goals, ownership, lifecycle status, OpenSpec path, and feedback references for each Change.
3. repository root `AGENTS.md` — mandatory agent behavior and handoff rules.
4. `openspec/config.yaml` — OpenSpec-injected project context and artifact rules.

## Planning → OpenSpec

```text
Chairman / ChatGPT planning
        ↓
_myplan_/ discussions + decisions + roadmap
        ↓
planning-manifest.yaml
        ↓
change-registry.yaml
        ↓
OpenSpec proposal/specs/design/tasks
        ↓
implementation + tests + evidence
```

Before creating or changing an OpenSpec Change, resolve the target Change in the manifest/registry and read its listed planning sources.

## OpenSpec → Planning feedback

Reverse flow is controlled; OpenSpec/implementation does not silently rewrite accepted planning history.

```text
Implementation / Review
        ↓
new finding / conflict / risk
        ↓
feedback/
        ↓
new governance decision if needed
        ↓
manifest / registry update
        ↓
OpenSpec update
```

See `feedback/README.md` for the required feedback record contract.

## What to read first for human review

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

### `feedback/`
Implementation/review findings returned to planning. Feedback is not automatically an accepted decision.

## Precedence

```text
Accepted OpenSpec / canonical specs
        ↓
Canonical OpenSpec design/contracts
        ↓
Verified implementation + tests/evidence
        ↓
Accepted architecture decisions
        ↓
Integrated roadmap
        ↓
Planning discussions / historical notes
        ↓
Conversation-only context
```

A proposed OpenSpec change may intentionally supersede current canonical specs only through explicit review/archive lifecycle.

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

```text
Change 001 — AI Company Organization Foundation
planning_status: ready_for_openspec
openspec_status: not_created
```

The Change should be created using the installed OpenSpec workflow, with planning provenance from `planning-manifest.yaml` and `change-registry.yaml`.

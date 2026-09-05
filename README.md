# Awin AI Company + UKS

This repository is the planning and implementation home for an **AI Company operating model** led by the Chairman, coordinated by **Awin** as CEO, with **Pick** as CIO and **UKS** as the Unified Knowledge System.

## Canonical organization

```text
Chairman / 董事長
        │ Goals / Decisions / Approval
        ▼
CEO — Awin
        │
        ├───────────────┬───────────────┐
        ▼               ▼               ▼
CIO — Pick            CTO             Other CXO
        │            [future]          [future]
        ▼
UKS — Unified Knowledge System
```

Canonical naming:

```text
Awin = CEO identity
Pick = CIO identity
UKS  = Unified Knowledge System
```

The organization uses bindings rather than hard-coded equivalence. A position can be rebound to a different identity without redesigning the company.

## Fundamental invariant

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

- **Position** — organizational office, such as Chairman, CEO, CIO, CTO.
- **Role** — reusable responsibility/capability profile.
- **Identity** — named human or AI actor bound to a position/role.
- **Agent** — software execution component acting for an identity/role.
- **Runtime** — Codex, Claude, Gemini, GPT, local model, or another provider/runtime.
- **System** — technical platform such as UKS.

## Operating principle

> **The Chairman manages an AI Company; the Chairman does not directly manage model runtimes.**

The durable business work unit is a **Mission**, not a chat prompt.

```text
Chairman Intent
      ↓
Mission
      ↓
CEO Awin understands / plans
      ↓
Assign Executive Owner
      ↓
Delegation Contract
      ↓
Execution
      ↓
Artifacts + Evidence
      ↓
Review
      ↓
Pass / Rework / Escalation
      ↓
CEO Synthesis
      ↓
Chairman
```

## Durable execution and handoff

Company state must survive conversation, agent, and runtime changes.

Canonical durable state includes:

```text
Mission state
OpenSpec Change
proposal.md
design.md
specs/
tasks.md
Git state / commits
source code
 tests
artifacts
evidence
audit events
```

Core rule:

> **OpenSpec Change owns durable implementation state; agents execute and advance that state.**

If Codex, Claude, Gemini, or another runtime reaches quota or becomes unavailable, work should resume from durable artifacts rather than prior chat history.

## UKS role

UKS is a **company system**, organizationally managed under CIO Pick. It is not the CIO identity and it does not define company governance.

Later UKS scope includes unified access to sources such as:

- GitHub
- Notion
- Google Drive
- NotebookLM

UKS will eventually provide source contracts, provenance, catalog/index, retrieval, authority/freshness handling, citations, and research support. Detailed UKS implementation begins only after the company orchestration foundations.

## Canonical roadmap

```text
001 — AI Company Organization Foundation
002 — Mission & Company Orchestration
003 — Agent Runtime & Execution Fabric
004 — UKS: Unified Knowledge System
005 — Awin CEO Orchestrator
006 — AI Company Control Center
```

### Change 001 — AI Company Organization Foundation

Organization, Department, Position, Role, Identity, Binding, Reporting, Authority, Governance, historical binding, audit foundation.

### Change 002 — Mission & Company Orchestration

Mission, Workstream, Task, Delegation Contract, Deliverable, Evidence, Review, Approval Gate, Escalation, Handoff, Mission State Machine.

### Change 003 — Agent Runtime & Execution Fabric

Role-to-agent resolution, runtime binding, provider selection, quota failover, resume/handoff, execution state, tool/runtime abstraction.

### Change 004 — UKS

Knowledge source and connector contracts, GitHub/Notion/Google Drive/NotebookLM integration, canonical knowledge representation, provenance, catalog/retrieval foundations.

### Change 005 — Awin CEO Orchestrator

Chairman intent interpretation, mission creation, executive owner selection, delegation planning, monitoring, escalation handling, cross-executive coordination, executive synthesis.

### Change 006 — AI Company Control Center

Organization chart, bindings, missions, tasks, executives, agents, runtime status, approvals, escalations, evidence, audit, cost/quota/status.

## Planning documents

`_myplan_/roadmap/2026-09-05_AI-Company-Orchestration_Integrated-Plan.md` is the current integrated planning baseline.

`_myplan_/decisions/` contains accepted architecture decisions.

`_myplan_/discussions/` preserves design rationale and alternatives, but canonical OpenSpec/specification artifacts take precedence once created.

## Document precedence

```text
Accepted OpenSpec / canonical specs
        ↓
Canonical design contracts
        ↓
Current implementation + tests
        ↓
Accepted architecture decisions
        ↓
Current integrated roadmap
        ↓
Planning discussions / historical notes
```

If older text conflicts with the current canonical naming or roadmap, the newer accepted decision/specification wins.

## Current status

Planning baseline established on **2026-09-05**.

Next recommended implementation planning target: **Change 001 — AI Company Organization Foundation**, followed by **Change 002 — Mission & Company Orchestration** before detailed UKS implementation.

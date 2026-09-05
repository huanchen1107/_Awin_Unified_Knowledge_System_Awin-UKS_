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

## OpenSpec planning-governance bridge

This repository deliberately separates planning history from executable change contracts:

```text
_myplan_                = planning, discussion, decisions, roadmap, feedback
openspec/               = executable change contracts and canonical specs
AGENTS.md               = permanent agent behavior / handoff contract
```

Agents MUST resolve planning before creating or modifying OpenSpec artifacts:

```text
_myplan_/planning-manifest.yaml
        ↓
_myplan_/change-registry.yaml
        ↓
Change-specific planning sources
        ↓
OpenSpec proposal/specs/design/tasks
        ↓
implementation + tests + evidence
```

Reverse synchronization is controlled rather than automatic rewriting:

```text
OpenSpec / implementation / review
        ↓
new finding / conflict / risk
        ↓
_myplan_/feedback/
        ↓
accepted new decision if required
        ↓
manifest / registry update
        ↓
OpenSpec update
```

Important files:

- `AGENTS.md` — mandatory multi-agent governance and handoff rules.
- `openspec/config.yaml` — OpenSpec project context injected into artifact generation.
- `_myplan_/planning-manifest.yaml` — machine-readable canonical planning map.
- `_myplan_/change-registry.yaml` — Change scopes, statuses, ownership, paths, and planning sources.
- `_myplan_/feedback/README.md` — controlled reverse-feedback protocol.
- `docs/OPENSPEC-BOOTSTRAP.md` — reproducible OpenSpec CLI setup for a new machine.

## OpenSpec CLI setup

OpenSpec is installed on each working machine; the package itself is not stored in this Git repository.

After clone, follow `docs/OPENSPEC-BOOTSTRAP.md`.

Typical commands:

```bash
npm install -g @fission-ai/openspec@latest
openspec init --tools antigravity,codex,claude,gemini
```

For an already initialized/upgraded working copy:

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

Preserve the custom project governance context in `openspec/config.yaml`.

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

`_myplan_/discussions/` preserves design rationale and alternatives, while `_myplan_/feedback/` receives implementation findings that may require a new planning decision.

Canonical OpenSpec/specification artifacts take precedence once accepted.

## Document precedence

```text
Accepted OpenSpec / canonical specs
        ↓
Canonical OpenSpec design/contracts
        ↓
Verified implementation + tests/evidence
        ↓
Accepted architecture decisions
        ↓
Current integrated roadmap
        ↓
Planning discussions / historical notes
        ↓
Conversation-only context
```

If older text conflicts with the current canonical naming or roadmap, the newer accepted decision/specification wins.

## Current status

Planning-governance bridge established on **2026-09-05**.

Current Change registry state:

```text
Change 001 — planning_status: ready_for_openspec
             openspec_status: not_created
             implementation_status: not_started
```

Next execution target: create **Change 001 — AI Company Organization Foundation** through the official OpenSpec workflow, using the planning sources declared in the manifest/registry.

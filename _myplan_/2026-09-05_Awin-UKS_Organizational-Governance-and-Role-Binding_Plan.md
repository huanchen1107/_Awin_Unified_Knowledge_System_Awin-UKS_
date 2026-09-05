# AI Company Organization Foundation — Governance & Binding Plan

**Date:** 2026-09-05  
**Status:** Canonical planning input for Change 001  
**Repository:** `huanchen1107/_Awin_Unified_Knowledge_System_Awin-UKS_`

## 1. Purpose

This document defines the **organizational foundation** of the Awin AI Company. It answers who exists, who occupies each position, who reports to whom, what authority is attached to a position, and how identities/runtimes can change without breaking the organization.

It deliberately does **not** define mission execution, UKS retrieval, RAG, connectors, or UI implementation.

## 2. Canonical organization

```text
Chairman / 董事長
        │
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

UKS is a **System**, not an Identity.

## 3. Fundamental domain separation

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

### Organization
The company/governance container.

### Position
A stable organizational office such as Chairman, CEO, CIO, CTO, CSO, CRO, or Chief of Staff.

### Role
A reusable responsibility/capability profile such as Executive Coordination, Knowledge Governance, Architecture, Builder, Reviewer, or Research.

### Identity
A named human, AI identity, team, service, or automation that may be bound to a position or role.

### Agent
A software execution component acting for a role/identity.

### Runtime
The current model/provider/runtime used by an agent, such as Codex, Claude, Gemini, GPT, or a local model.

### System
A technical platform operated by the organization, such as UKS.

## 4. Initial position–identity bindings

```yaml
bindings:
  - position_id: chairman
    identity_id: owner
    effective_from: 2026-09-05

  - position_id: ceo
    identity_id: awin
    effective_from: 2026-09-05

  - position_id: cio
    identity_id: pick
    effective_from: 2026-09-05
```

These are bindings, not permanent equivalences.

Future examples:

```text
CEO → another identity
Chief of Staff → Awin
CIO → another identity
```

The company structure should continue to function after rebinding.

## 5. Reporting structure

Initial reporting chain:

```text
Chairman
   ↓
CEO
   ↓
CIO / CTO / other executives
```

Reporting relationships belong to **Positions**, not to AI provider names.

## 6. Position-based invocation

The Chairman should be able to communicate using organizational titles.

Example:

```text
「叫資訊長查一下。」
      ↓
Position Registry: CIO
      ↓
Binding Registry: Pick
      ↓
current authorized agent/runtime
```

The Chairman should not need to know whether Pick is currently powered by Claude, Gemini, GPT, or another runtime.

## 7. Role model

Roles are reusable capability profiles and should not be confused with positions.

Examples:

```text
Executive Coordination
Knowledge Governance
Software Architecture
Implementation
Review
Security Review
Research
Teaching
Operations
```

A Position may carry multiple Roles; a Role may be fulfilled by different identities/agents over time.

## 8. Runtime separation and failover

Runtime/provider details are not organizational identity.

Example:

```text
Position : CIO
Identity : Pick
Agent    : CIO Executive Agent
Runtime  : Claude
```

may later become:

```text
Position : CIO
Identity : Pick
Agent    : CIO Executive Agent
Runtime  : Gemini
```

Pick remains Pick.

Runtime selection/fallback is implemented later in Change 003, but Change 001 must preserve the abstraction boundary.

## 9. Authority model

Authority belongs primarily to **Positions + Policies**.

Examples:

```yaml
ceo:
  may:
    - interpret_chairman_intent
    - coordinate_executives
    - delegate_within_policy
    - resolve_cross-domain ownership
  may_not_without_chairman:
    - rewrite_company_governance

cio:
  may:
    - govern_information_domain
    - manage_systems_assigned_to_cio
    - delegate_information work within policy
```

A runtime does not gain authority merely because it can technically perform an action.

## 10. Delegation boundary

Change 001 defines **who may delegate to whom** at an organizational-policy level.

The detailed runtime work object — Mission, Workstream, Task, Delegation Contract, Evidence, Approval, Escalation — belongs to **Change 002**.

Therefore Change 001 may define:

```text
Chairman may delegate to CEO
CEO may delegate to executives
CIO may delegate within its authorized domain
```

but it should not yet implement Mission orchestration.

## 11. Historical binding and audit

Bindings must be temporal and auditable.

```yaml
binding_event:
  id: bind-evt-001
  position_id: cio
  identity_id: pick
  effective_from: 2026-09-05T00:00:00+08:00
  effective_to: null
  status: active
  changed_by_position: chairman
```

When a binding changes:

```text
old binding → closed
new binding → active
```

This enables historical questions such as:

- Who was CIO when this decision was made?
- Which identity acted under CEO authority?
- Which runtime executed the task at that time?

## 12. Proposed bootstrap registry

```text
organization/
├── positions/
│   ├── chairman.yaml
│   ├── ceo.yaml
│   └── cio.yaml
├── roles/
│   ├── executive_coordination.yaml
│   └── knowledge_governance.yaml
├── identities/
│   ├── owner.yaml
│   ├── awin.yaml
│   └── pick.yaml
├── bindings/
│   └── position_bindings.yaml
├── policies/
│   ├── authority.yaml
│   └── delegation.yaml
└── history/
    └── binding_events.jsonl
```

A database may later supplement these files. Human-readable configuration remains useful as bootstrap and governance evidence.

## 13. Change 001 scope

### In scope

```text
Organization
Department abstraction
Position
Role
Identity
Position–Identity Binding
Reporting Line
Authority Policy
Delegation Policy boundary
Historical Binding
Audit foundation
Runtime-binding abstraction only as a portability contract
```

### Out of scope

```text
Mission / Workstream / Task execution
Delegation Contract lifecycle
Evidence / Review / Approval / Escalation workflow
Provider selection and quota failover implementation
UKS connectors / RAG / indexing / retrieval
NotebookLM integration
CEO autonomous orchestration
Control Center UI
```

## 14. Change dependency

```text
001 Organization Foundation
        ↓
002 Mission & Company Orchestration
        ↓
003 Agent Runtime & Execution Fabric
        ↓
004 UKS
        ↓
005 Awin CEO Orchestrator
        ↓
006 AI Company Control Center
```

## 15. Acceptance intent for Change 001

After Change 001 the system should be able to model and answer:

```text
Who is CEO? → Awin
Who is CIO? → Pick
Who does CIO report to? → CEO
What authority belongs to CIO? → resolve policy
Can CIO identity be replaced? → yes, via binding change
Was Pick CIO on a historical date? → resolve binding history
```

It should **not yet** be expected to execute a full Mission or search UKS.

## 16. Planning rule

> **Change 001 builds the company structure. Change 002 teaches the company how to work. Change 004 builds the UKS information system operated by the company.**

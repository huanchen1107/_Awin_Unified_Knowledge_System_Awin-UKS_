# AI Company Orchestration — Canonical Integrated Roadmap

**Date:** 2026-09-05  
**Status:** Canonical planning baseline  
**Repository:** `huanchen1107/_Awin_Unified_Knowledge_System_Awin-UKS_`

## 1. Strategic direction

This project is an **AI Company operating model**, not merely an agent framework, chatbot, or RAG application.

Core principle:

> **The Chairman manages an AI Company; the Chairman does not directly manage model runtimes.**

The company must support durable missions, explicit delegation, evidence-backed execution, review, escalation, policy-based approval, provider/runtime replacement, and auditable handoff.

## 2. Canonical organization

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

## 3. Fundamental invariant

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

Example:

```text
Position : CIO
Identity : Pick
Role     : Information Executive
Agent    : CIO Executive Agent
Runtime  : replaceable
System   : UKS
```

This separation is required for governance, failover, portability, historical audit, and organizational continuity.

## 4. Company operating loop

The durable business work unit is a **Mission**, not a chat prompt.

```text
Chairman Intent
      ↓
Mission
      ↓
CEO Understand / Plan
      ↓
Assign Executive Owner
      ↓
Delegation Contract
      ↓
Workstream / Task Execution
      ↓
Artifacts + Evidence
      ↓
Review
   ┌──┴─────────────┐
   ↓                ↓
 PASS          REWORK / PROBLEM
   │                │
   │          Escalation if needed
   └────────┬───────┘
            ↓
       CEO Synthesis
            ↓
        Chairman
```

Awin should not automatically do every task. As CEO, Awin should interpret intent, establish/shape Missions, choose ownership, delegate, monitor, coordinate, handle escalation, and return an executive synthesis.

## 5. Durable Mission model

Conceptual structure:

```text
Mission
├── Workstream
│   ├── Task
│   ├── Task
│   └── Task
├── Workstream
│   └── Task
├── Deliverables
├── Evidence
├── Reviews
├── Approvals
└── Audit Events
```

A Mission must survive changes in:

```text
conversation
agent
runtime
provider
machine/device
```

## 6. Delegation contract

Delegation is an explicit company object, not a casual agent call.

Example:

```yaml
delegation:
  mission_id: M-2026-001
  from_position: ceo
  to_position: cio
  objective: "Find existing Lesson 1–3 materials"
  expected_output: evidence_package
  authority:
    read: true
    modify: false
  return_to_position: ceo
```

Resolution occurs at execution time:

```text
Target Position
      ↓
Position–Identity Binding
      ↓
Role / Agent Resolution
      ↓
Runtime Resolution
```

## 7. Runtime independence

Runtime/provider belongs below the organization.

```text
Position
   ↓
Identity
   ↓
Role
   ↓
Agent
   ↓
Runtime Resolver
   ↓
Codex / Claude / Gemini / GPT / Local
```

Example failover:

```text
Builder Role
├── preferred → Codex
├── fallback  → Claude
└── fallback  → Gemini
```

Provider failure or quota exhaustion must not change Mission ownership, organizational authority, or identity.

## 8. Durable handoff and OpenSpec

Company work must not depend on previous chat history as canonical state.

Preferred durable state:

```text
Mission state
OpenSpec Change
proposal.md
design.md
specs/
tasks.md
Git working tree / commits
source code
tests
artifacts
evidence
audit/event log
```

Core rule:

> **OpenSpec Change owns durable implementation state; agents execute and advance that state.**

A replacement agent/runtime resumes by reading durable state and validating current repository/test evidence.

## 9. Evidence and review

Execution should return more than a result.

```text
Result
 + Artifact
 + Evidence
 + Validation
      ↓
    Review
```

Evidence may include:

- commit SHA;
- test results;
- source references;
- diff;
- generated artifact;
- validation output;
- provenance records;
- screenshot or runtime evidence where appropriate.

Review should compare artifacts against acceptance criteria, not merely trust the executor's self-report.

## 10. Approval and escalation

Approval should be policy-driven.

Conceptual levels:

```text
L0 Autonomous        — routine read/analyze/non-destructive work
L1 Manager Approval  — routine changes within delegated scope
L2 Executive Approval— architecture/cross-domain/high-impact changes
L3 CEO Approval      — major implementation/company-wide decisions
L4 Chairman Approval — strategy/governance/destructive/high-risk/high-cost actions
```

Escalation triggers may include:

```text
insufficient authority
conflicting canonical sources
ambiguous strategic intent
architecture conflict
low confidence + high impact
security/privacy concern
destructive action
cost/quota threshold
reviewer rejection
```

Escalation path:

```text
Worker → Manager/Role Owner → Executive → CEO Awin → Chairman if required
```

## 11. Canonical roadmap

### Change 001 — AI Company Organization Foundation

Recommended id:

```text
001-ai-company-organization-foundation
```

Scope:

```text
Organization
Department
Position
Role
Identity
Binding
Reporting Line
Authority
Governance
Historical Binding
Audit Foundation
```

Output: a stable company/governance model independent of agents and runtimes.

### Change 002 — Mission & Company Orchestration

Recommended id:

```text
002-mission-company-orchestration
```

Scope:

```text
Mission
Workstream
Task
Delegation Contract
Deliverable
Evidence Package
Review
Approval Gate
Escalation
Handoff / Resume
Mission State Machine
Orchestration audit events
Completion criteria
```

Output: a durable company work lifecycle.

### Change 003 — Agent Runtime & Execution Fabric

Recommended id:

```text
003-agent-runtime-execution-fabric
```

Scope:

```text
Role-to-agent resolution
Agent/runtime binding
Provider/model selection
Capability matching
Runtime health
Quota/failure fallback
Resume/handoff protocol
Execution state
Tool abstraction
```

Output: resilient execution independent of any single model provider.

### Change 004 — UKS: Unified Knowledge System

Recommended id:

```text
004-uks-unified-knowledge-system
```

Organizational owner:

```text
CIO Position → Pick Identity
                  │ manages
                  ▼
                 UKS
```

Initial scope:

```text
Knowledge Source Registry
Connector Contracts
GitHub
Notion
Google Drive
NotebookLM
Canonical Knowledge Representation
Provenance
Catalog / retrieval foundation
```

UKS is a company system, not the organizational center.

### Change 005 — Awin CEO Orchestrator

Recommended id:

```text
005-awin-ceo-orchestrator
```

Scope:

```text
Chairman intent interpretation
Mission creation/shaping
Executive owner selection
Delegation planning
Mission monitoring
Cross-executive coordination
Escalation handling
Executive synthesis
Chairman reporting
```

Output: Awin behaves as the company's CEO rather than a generic assistant.

### Change 006 — AI Company Control Center

Recommended id:

```text
006-ai-company-control-center
```

Potential scope:

```text
Organization chart
Current bindings
Mission portfolio
Workstreams/tasks
Approvals
Escalations
Evidence
Executives/agents
Runtime health
Quota/cost/status
Audit timeline
```

Output: Chairman-facing control plane for observing and governing the AI Company.

## 12. Dependency direction

```text
Organization Foundation
        ↓
Mission & Orchestration Contracts
        ↓
Execution Fabric
        ↓
Company Systems (UKS, future systems)
        ↓
CEO Orchestrator
        ↓
Control Center
```

Rules:

- Organization must not depend on UKS internals.
- Mission semantics must not depend on a specific AI provider.
- Runtime fabric must not own company authority.
- UKS must not redefine governance.
- Awin CEO logic consumes the contracts established by Changes 001–004.
- UI observes/controls canonical state; it must not become a second source of truth.

## 13. Example mission

Chairman request:

> Design Lesson 3 AI Red Team curriculum.

Possible flow:

```text
Chairman
   ↓
Mission
   ↓
CEO Awin
   ├── information retrieval → CIO Pick / UKS
   ├── curriculum architecture → appropriate role/executive
   ├── implementation → Builder
   └── validation → Reviewer / Security role
           ↓
    Artifacts + Evidence
           ↓
     Review / Escalation
           ↓
        CEO Awin
           ↓
  Executive Synthesis
           ↓
       Chairman
```

The worker runtime may change without altering the organizational Mission structure.

## 14. Canonical planning decisions

1. This is an AI Company operating model.
2. Chairman operates through goals, decisions, approval, and exceptions.
3. Awin is initially bound to CEO.
4. Pick is initially bound to CIO.
5. UKS is a System managed under CIO scope.
6. Position, Role, Identity, Agent, Runtime, and System are separate.
7. Position/identity bindings are temporal and replaceable.
8. Mission is the durable work unit.
9. Delegation is explicit and auditable.
10. Execution returns artifacts and evidence.
11. Review evaluates acceptance criteria.
12. Important unresolved issues escalate instead of being guessed through.
13. Approval is policy-based.
14. Runtime/provider failover does not redefine organizational responsibility.
15. Durable artifacts, OpenSpec, Git, tests, and audit state support multi-agent continuation.
16. UKS begins at Change 004, not Change 002.
17. Awin's autonomous CEO behavior is layered on top of the foundational contracts rather than hard-coded into them.

## 15. Immediate next planning work

Proceed in order:

```text
1. Design OpenSpec Change 001 in detail.
2. Validate Organization / Position / Role / Identity / Binding invariants.
3. Then design Change 002 Mission state machine and Delegation Contract.
4. Only after those foundations, design runtime execution and UKS implementation.
```

For Change 002, the next detailed discussion should define:

```text
Mission states
Workstream/task hierarchy
Delegation Contract schema
Evidence Package schema
Review/rework semantics
Approval hooks
Escalation events
handoff/resume semantics
completion criteria
immutable audit history
```

## 16. Document precedence

```text
Accepted OpenSpec / canonical specifications
        ↓
Canonical design contracts
        ↓
Current implementation + tests
        ↓
Accepted architecture decisions
        ↓
This integrated roadmap
        ↓
Planning discussions / historical notes
```

Newer accepted specifications may intentionally supersede this roadmap; superseding relationships should be documented explicitly.

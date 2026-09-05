# AI Company Orchestration — Integrated Planning Baseline

**Date:** 2026-09-05  
**Repository:** `huanchen1107/_Awin_Unified_Knowledge_System_Awin-UKS_`  
**Status:** Planning baseline  
**Scope:** Company operating model and orchestration roadmap before UKS implementation

---

## 1. Strategic Direction

The project is not merely a collection of agents. It is an **AI Company** with organizational structure, executive responsibility, delegated missions, durable execution state, review, escalation, and governance.

The Chairman should operate the company through goals and decisions rather than by manually selecting individual model providers or worker agents.

Core principle:

> **The Chairman manages an AI Company; the Chairman does not directly manage model runtimes.**

---

## 2. Current Canonical Organization

```text
Chairman / 董事長
        │
        │ Goals / Decisions
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
Awin = CEO Identity
Pick = CIO Identity
UKS  = Unified Knowledge System
```

The relationship is binding-based rather than hard-coded.

---

## 3. Fundamental Domain Separation

The organization must preserve the invariant:

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

Definitions:

- **Position** — organizational office, such as Chairman, CEO, CIO, CTO.
- **Role** — reusable responsibility/capability profile.
- **Identity** — named actor, human or AI, currently bound to a position or role.
- **Agent** — software execution component acting for an identity/role.
- **Runtime** — model/provider/runtime such as Codex, Claude, Gemini, GPT, or local model.
- **System** — technical platform or resource such as UKS.

Example:

```text
Position : CIO
Identity : Pick
Role     : Information Executive
System   : UKS
Runtime  : interchangeable
```

---

## 4. Company Operating Model

The preferred company lifecycle is:

```text
Chairman Intent
      ↓
Mission
      ↓
CEO Understands
      ↓
Plan / Decompose
      ↓
Assign Executive Owner
      ↓
Delegation
      ↓
Execution
      ↓
Artifacts + Evidence
      ↓
Review
      ↓
Pass OR Escalation
      ↓
CEO Synthesis
      ↓
Chairman
```

Awin, as CEO, should not automatically execute every task directly. Awin should interpret the Chairman's intent, establish a mission, determine ownership, delegate work, monitor progress, handle escalation, and return an executive synthesis.

---

## 5. Mission as the Primary Work Unit

A user prompt is not the durable company work unit. The durable unit should be a **Mission**.

Example conceptual model:

```yaml
mission:
  id: M-2026-001
  requested_by:
    position: chairman
  owner:
    position: ceo
  goal: "Complete Lesson 3 AI Red Team curriculum"
  status: active
  success_criteria:
    - curriculum complete
    - labs complete
    - slides complete
    - instructor notes complete
    - security boundaries documented
```

A Mission may contain:

```text
Mission
 ├── Workstream
 │    ├── Task
 │    ├── Task
 │    └── Task
 ├── Workstream
 │    └── Task
 └── Deliverables
```

The mission should survive agent, model, runtime, and conversation changes.

---

## 6. Delegation Contract

Delegation should be explicit and auditable rather than equivalent to casually invoking another agent.

Example:

```yaml
delegation:
  from: CEO
  to: CIO
  mission: M-2026-001
  objective: "Find all existing Lesson 1–3 materials"
  expected_output: "Evidence Package"
  authority:
    read_sources: true
    modify_sources: false
  return_to: CEO
```

Conceptual flow:

```text
Awin / CEO
   ↓
Delegation Contract
   ↓
Pick / CIO
   ↓
Execution
   ↓
Evidence / Artifact
   ↓
Return to CEO
```

Delegation should normally occur between organizational positions, with the current identity binding resolved at execution time.

---

## 7. Runtime and Provider Separation

Model vendors/providers are execution resources rather than organizational offices.

The preferred execution chain is:

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
Codex / Claude / Gemini / GPT / Local Model
```

If one runtime becomes unavailable or reaches quota limits, the task should continue through another runtime without redefining the position, identity, mission, or delegated responsibility.

Example:

```text
Builder Role
├── preferred → Codex
├── fallback  → Claude
└── fallback  → Gemini
```

---

## 8. Durable Handoff and OpenSpec

Company work must not depend on prior chat history as its canonical state.

Preferred durable handoff sources include:

```text
Mission state
OpenSpec Change
proposal.md
design.md
specs/
tasks.md
Git working tree
commits
source code
tests
artifacts
audit/event logs
```

Core principle:

> **OpenSpec Change owns durable implementation state; agents execute and advance that state.**

A runtime change should look like:

```text
Codex quota exhausted
        ↓
Read canonical artifacts
        ↓
Claude continues
        ↓
State persists
        ↓
Gemini may continue later
```

No agent should require the entire prior conversation in order to resume safely.

---

## 9. Review and Evidence

Execution should produce not only a result but also evidence.

Conceptual package:

```text
Task Result
   +
Artifact
   +
Evidence
   +
Validation
   ↓
Review
```

Evidence may include:

- commit SHA
- test results
- source references
- generated files
- diffs
- screenshots
- validation output
- provenance records

The Reviewer should evaluate the artifact against mission/task acceptance criteria rather than simply accepting an agent's self-report.

---

## 10. Escalation Model

Agents and executives must not silently guess through important conflicts or insufficient authority.

Typical escalation triggers:

- insufficient authority
- conflicting canonical sources
- ambiguous strategic intent
- architecture conflict
- low-confidence decision with high impact
- cost/compute threshold exceeded
- destructive operation
- security/privacy policy conflict
- reviewer rejection

Conceptual path:

```text
Worker
  ↓ unresolved
Manager / Role Owner
  ↓
Executive
  ↓
CEO — Awin
  ↓ when governance decision required
Chairman
```

Example:

```text
Canonical design.md conflicts with a new Chairman instruction
        ↓
Pick / responsible executive creates Conflict Report
        ↓
Awin determines whether policy can resolve it
        ↓
Chairman decision if necessary
```

---

## 11. Approval Gates

Not every task should require Chairman approval. Approval should be policy-based.

Suggested conceptual levels:

```text
L0 — Autonomous
Read/search/analyze/summarize/non-destructive routine work

L1 — Manager Approval
Routine artifact updates within delegated scope

L2 — Executive Approval
Architecture, cross-domain, or higher-impact changes

L3 — CEO Approval
Major implementation or company-wide operational decisions

L4 — Chairman Approval
Governance changes, strategic changes, destructive/high-cost/high-risk actions
```

These levels are planning concepts and should later be formalized through policy rather than hard-coded assumptions.

---

## 12. AI Company Operating Loop

```text
              CHAIRMAN
                  │
                Intent
                  ▼
               MISSION
                  │
                  ▼
             CEO — Awin
                  │
            Plan / Delegate
                  ▼
        ┌──────────────────┐
        │ Executive Layer  │
        │ CIO / CTO / ...  │
        └────────┬─────────┘
                 │
              Delegate
                 ▼
        ┌──────────────────┐
        │ Manager / Agent  │
        │ Specialist Layer │
        └────────┬─────────┘
                 │
              Execute
                 ▼
        Runtime / Tools / Systems
                 │
                 ▼
              Evidence
                 │
                 ▼
              Review
            ┌────┴────┐
            │         │
          PASS     PROBLEM
            │         │
            │     Escalation
            │         │
            └────┬────┘
                 ▼
              CEO Awin
                 │
          Executive Summary
                 ▼
              CHAIRMAN
```

This operating loop is the current canonical planning direction for AI Company Orchestration.

---

## 13. Revised Change Roadmap

The prior roadmap that placed UKS immediately in Change 002 is superseded by the company-orchestration-first sequence below.

### Change 001 — AI Company Organization Foundation

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
Historical binding / audit foundation
```

Initial canonical bindings:

```text
Chairman → Owner
CEO      → Awin
CIO      → Pick
```

### Change 002 — Mission & Company Orchestration

Scope:

```text
Mission
Workstream
Task
Delegation Contract
Deliverable
Evidence
Review
Approval Gate
Escalation
Handoff
Mission State Machine
```

This change defines how the company actually performs work.

### Change 003 — Agent Runtime & Execution Fabric

Scope:

```text
Role-to-agent resolution
Runtime binding
Provider selection
Runtime fallback
Quota failover
Resume/handoff protocol
Execution state
Tool/runtime abstraction
```

Providers such as Codex, Claude, Gemini, GPT, and local models live here rather than in the organization model.

### Change 004 — UKS: Unified Knowledge System

Scope begins the information system managed organizationally by CIO Pick.

Initial areas:

```text
Knowledge sources
GitHub
Notion
Google Drive
NotebookLM
source/connector contracts
canonical knowledge representation
provenance
```

UKS is a company system, not the organizational center of the company.

### Change 005 — Awin CEO Orchestrator

Scope:

```text
Chairman intent interpretation
Mission creation
Executive owner selection
Delegation planning
Mission monitoring
Escalation handling
Cross-executive coordination
Executive synthesis
```

### Change 006 — AI Company Control Center

Potential UI/control-plane scope:

```text
Organization chart
Current bindings
Missions
Workstreams/tasks
Executives/agents
Runtime status
Approvals
Escalations
Evidence
Audit
Cost/quota/status
```

---

## 14. Architectural Dependency Direction

Preferred dependency order:

```text
Organization Foundation
        ↓
Mission & Orchestration Contracts
        ↓
Execution Fabric
        ↓
Company Systems such as UKS
        ↓
CEO Orchestrator
        ↓
Control Center / UI
```

The organization must not depend on UKS implementation details, and UKS should not define company governance.

---

## 15. Example Mission

Chairman request:

> Design Lesson 3 AI Red Team curriculum.

Possible organizational execution:

```text
Chairman
   ↓
Mission: Lesson 3 AI Red Team
   ↓
CEO — Awin
   ├── delegates information retrieval → CIO Pick / UKS
   ├── delegates curriculum architecture → appropriate executive/role
   ├── delegates lab implementation → Builder
   └── delegates validation → Reviewer / Security role
            ↓
       Evidence + Artifacts
            ↓
       Review / Escalation
            ↓
         CEO — Awin
            ↓
      Executive Synthesis
            ↓
         Chairman
```

The specific worker runtime may change without changing this mission structure.

---

## 16. Current Accepted Planning Decisions

1. Awin AI Company is an organizational operating model, not merely an agent framework.
2. Chairman communicates primarily through goals, decisions, approvals, and exceptions.
3. Awin is the initially bound CEO identity.
4. Pick is the initially bound CIO identity.
5. UKS is a company system managed under the CIO scope, not an identity.
6. Position, Role, Identity, Agent, Runtime, and System remain separate concepts.
7. Mission is the preferred durable business/work unit.
8. Delegation should be explicit and auditable.
9. Execution should return artifacts and evidence.
10. Review is independent from execution wherever practical.
11. Important unresolved conditions should escalate rather than be guessed through.
12. Approval gates should be policy-driven.
13. Model/runtime failover must not redefine mission ownership or organizational responsibility.
14. OpenSpec/Git/test/artifact state should enable multi-agent continuation without relying on chat history.
15. UKS implementation is deferred until after company orchestration foundations.
16. Revised roadmap is Change 001 through Change 006 as defined in this document.

---

## 17. Next Planning Target

The next detailed architecture discussion should focus on **Change 002 — Mission & Company Orchestration**, especially the lifecycle:

```text
Create Mission
   ↓
Plan
   ↓
Delegate
   ↓
Execute
   ↓
Collect Evidence
   ↓
Review
   ↓
Approve / Rework / Escalate
   ↓
Complete
   ↓
Archive / Learn
```

Topics to define before implementation include:

- mission state machine
- task/workstream hierarchy
- delegation contract schema
- evidence package schema
- approval policy hooks
- escalation event schema
- handoff/resume semantics
- mission completion criteria
- immutable audit history

This should be completed before returning to detailed UKS system architecture.

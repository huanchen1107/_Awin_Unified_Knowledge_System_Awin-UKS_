# Change 001 / Change 002 Scope Boundary — AI Company First

**Date:** 2026-09-05  
**Status:** Accepted planning decision, revised to match the canonical AI Company roadmap

## Core decision

The previous idea that `Change 002 = UKS System Foundation` is superseded.

The current canonical sequence is:

```text
Change 001 = AI Company Organization Foundation
Change 002 = Mission & Company Orchestration
Change 003 = Agent Runtime & Execution Fabric
Change 004 = UKS — Unified Knowledge System
Change 005 = Awin CEO Orchestrator
Change 006 = AI Company Control Center
```

The reason is architectural: the company must first define **who exists**, then **how work moves through the company**, then **how agents/runtimes execute**, before building a department system such as UKS.

## Change 001 — AI Company Organization Foundation

Recommended change id:

```text
001-ai-company-organization-foundation
```

### Goal

Define:

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

Canonical initial bindings:

```text
Chairman → Owner
CEO      → Awin
CIO      → Pick
```

Canonical distinction:

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

### Change 001 answers

```text
Who is CEO?
Who is CIO?
Who reports to whom?
What authority belongs to a position?
Who is currently bound to a position?
How can an identity be replaced without changing the organization?
What was the historical binding at a given time?
```

### Explicitly out of scope

```text
Mission lifecycle
Task execution
Delegation Contract lifecycle
Evidence / Review / Approval / Escalation
Provider selection and failover implementation
UKS connectors / retrieval / RAG
Awin autonomous CEO orchestration
Company Control Center UI
```

## Change 002 — Mission & Company Orchestration

Recommended change id:

```text
002-mission-company-orchestration
```

### Goal

Define **how the AI Company performs work** after the organization exists.

The durable work unit is a **Mission**, not a chat prompt.

```text
Chairman Intent
      ↓
Mission
      ↓
Plan / Decompose
      ↓
Assign Owner
      ↓
Delegation Contract
      ↓
Execute
      ↓
Artifacts + Evidence
      ↓
Review
      ↓
Approve / Rework / Escalate
      ↓
Complete
```

### Change 002 scope

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
Handoff
Resume semantics
Mission State Machine
Audit events for orchestration
Completion criteria
```

### Change 002 does not implement

```text
Claude/Codex/Gemini provider selection internals
Quota failover engine
UKS connectors/search
NotebookLM research
CEO autonomous mission creation from free-form Chairman input
Full Control Center UI
```

These are later changes.

## Change 003 — Agent Runtime & Execution Fabric

Purpose:

```text
Role-to-agent resolution
Agent runtime binding
Provider/model selection
Quota/failure fallback
Resume/handoff protocol
Execution state
Tool abstraction
Runtime health/capability resolution
```

The organization and Mission should survive runtime replacement.

## Change 004 — UKS

UKS begins here.

Canonical organizational relationship:

```text
CIO Position → Pick Identity
                  │ manages
                  ▼
      UKS — Unified Knowledge System
```

UKS is a **System**, not the CIO identity.

Initial areas may include:

```text
Knowledge Source Registry
Connector Contracts
GitHub
Notion
Google Drive
NotebookLM
Canonical Knowledge Representation
Provenance
Catalog / retrieval foundations
```

## Change 005 — Awin CEO Orchestrator

Awin becomes the active CEO orchestration layer here:

```text
Chairman Intent
      ↓
Awin interprets
      ↓
Create/shape Mission
      ↓
Select executive owner
      ↓
Delegate / monitor
      ↓
Handle escalation
      ↓
Coordinate executives
      ↓
Executive synthesis
      ↓
Chairman
```

## Change 006 — AI Company Control Center

Potential scope:

```text
Organization chart
Position/identity bindings
Mission portfolio
Workstreams/tasks
Approvals
Escalations
Evidence
Executives/agents
Runtime status
Quota/cost/status
Audit timeline
```

## Boundary rules

Use these rules when deciding where a feature belongs:

> **Who exists / who has authority / who reports to whom? → Change 001.**

> **How does company work move from intent to completion? → Change 002.**

> **Which agent/model/runtime executes and how does it fail over? → Change 003.**

> **Where is knowledge and how is it represented/retrieved? → Change 004.**

> **How does Awin autonomously behave as CEO? → Change 005.**

> **How does the Chairman observe/control the company? → Change 006.**

## Examples

| Question / feature | Change |
|---|---:|
| Who is currently CEO? | 001 |
| Rebind CIO from Pick to another identity | 001 |
| Mission state machine | 002 |
| Delegation Contract | 002 |
| Review / evidence / escalation | 002 |
| Codex quota fallback to Claude | 003 |
| Resume execution on Gemini | 003 |
| Connect GitHub / Notion / Drive | 004 |
| Knowledge provenance | 004 |
| Awin decides which executive owns a new Mission | 005 |
| Company dashboard | 006 |

## Planning conclusion

```text
001 Build the company structure.
002 Define how the company works.
003 Define how AI execution runs reliably.
004 Build UKS as a company system.
005 Make Awin operate as CEO.
006 Give the Chairman a control center.
```

This document supersedes the earlier Change 001/002 interpretation that placed UKS directly in Change 002.

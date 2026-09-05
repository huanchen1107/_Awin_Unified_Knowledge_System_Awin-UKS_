# Design Discussion — AI Company Organization, Identity Binding, and Orchestration Direction

**Date:** 2026-09-05  
**Status:** Historical design rationale aligned to current canonical decisions  
**Purpose:** Explain why the project uses an AI Company model and why Position, Identity, Agent, Runtime, and System are separated.

## 1. How the design evolved

The project originally began as a unified knowledge concept spanning GitHub, Notion, Google Drive, and NotebookLM. During discussion, the higher-level requirement became clearer: the Chairman needs an **AI Company**, not merely a search system or collection of agents.

The knowledge system therefore becomes one company system rather than the architecture center.

Current organization:

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

## 2. Why hard-coded names were rejected

A naive model would define:

```text
Awin = CEO
Pick = CIO
```

as permanent facts.

That is too rigid. Positions must survive identity replacement.

Accepted model:

```text
Position ── Binding ──> Identity
```

Initial configuration:

```text
CEO Position ──> Awin Identity
CIO Position ──> Pick Identity
```

Awin and Pick are initial bindings, not permanent organizational definitions.

## 3. The central invariant

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

This separation solves multiple problems:

- organizational continuity when an identity changes;
- runtime/provider portability;
- quota/failure failover;
- auditability;
- historical reconstruction;
- natural-language commands by title;
- systems remaining separate from the executives who manage them.

## 4. Why Pick replaced the old CIO identity name

Earlier discussion temporarily used `UKS` as the CIO identity. That created a collision because UKS also means **Unified Knowledge System**.

The accepted correction is:

```text
Awin = CEO identity
Pick = CIO identity
UKS  = Unified Knowledge System
```

Therefore:

```text
CIO — Pick
   │ manages
   ▼
UKS
```

Any older text saying `CIO — UKS` is historical and superseded.

## 5. Position-based invocation

The Chairman should speak in organizational language rather than runtime/provider language.

Example:

```text
「叫資訊長查一下。」
       ↓
resolve CIO Position
       ↓
resolve current binding → Pick
       ↓
resolve authorized role/agent
       ↓
resolve runtime
```

This allows the company to evolve without forcing the Chairman to remember internal agent names or model providers.

## 6. Why runtime/provider is not identity

Example:

```text
Position : CIO
Identity : Pick
Agent    : CIO Executive Agent
Runtime  : Claude
```

If Claude becomes unavailable:

```text
Runtime → Gemini
```

Nothing else needs to change.

The same principle applies to workers:

```text
Builder Role
├── preferred → Codex
├── fallback  → Claude
└── fallback  → Gemini
```

The task, Mission, role ownership, and organizational authority remain stable.

## 7. From organization to company operation

A second design correction emerged: it is not enough to define positions and bindings. The company also needs a durable way to perform work.

The durable work unit should be a **Mission**, not a prompt.

```text
Chairman Intent
      ↓
Mission
      ↓
CEO plans / decomposes
      ↓
assign executive owner
      ↓
delegation
      ↓
execution
      ↓
artifacts + evidence
      ↓
review
      ↓
pass / rework / escalation
      ↓
CEO synthesis
      ↓
Chairman
```

This realization moved UKS later in the roadmap.

## 8. Why UKS is not Change 002 anymore

Earlier planning placed UKS directly after Organization Foundation. That sequence was revised because three foundations are needed first:

```text
1. Who exists?                → Organization
2. How does company work?     → Mission Orchestration
3. How does AI execute?       → Runtime Fabric
4. What systems does it use?  → UKS and others
```

Therefore the current roadmap is:

```text
001 AI Company Organization Foundation
002 Mission & Company Orchestration
003 Agent Runtime & Execution Fabric
004 UKS — Unified Knowledge System
005 Awin CEO Orchestrator
006 AI Company Control Center
```

## 9. Mission and delegation rationale

Awin should not simply call arbitrary agents. Delegation should be explicit and auditable.

Conceptually:

```text
CEO Awin
   ↓
Delegation Contract
   ↓
Executive Position
   ↓
current Identity Binding
   ↓
Agent / Runtime
   ↓
Artifact + Evidence
   ↓
Review / Return
```

This enables authority boundaries, rework, escalation, handoff, and accountability.

## 10. Durable handoff rationale

The user's multi-agent workflow regularly uses different providers. Work must therefore survive model quota limits and conversation boundaries.

Canonical continuation state should be persisted in artifacts such as:

```text
Mission state
OpenSpec Change
proposal.md
design.md
specs/
tasks.md
Git state / commits
code
tests
artifacts
evidence
audit events
```

Core rule:

> **OpenSpec Change owns durable implementation state; agents execute and advance that state.**

Chat history is context, not the canonical implementation state.

## 11. Review, approval, and escalation

Agents should not silently guess through high-impact conflicts.

Typical escalation triggers include:

- insufficient authority;
- conflicting canonical sources;
- architecture conflicts;
- low-confidence/high-impact choices;
- destructive operations;
- security/privacy concerns;
- cost/quota thresholds;
- reviewer rejection.

Potential path:

```text
Worker
  ↓ unresolved
Manager / Role Owner
  ↓
Executive
  ↓
CEO Awin
  ↓ if strategic/governance decision required
Chairman
```

Approval should be policy-driven rather than requiring Chairman approval for every action.

## 12. Accepted design decisions

1. The project is an **AI Company operating model**, not merely an agent framework or RAG application.
2. Chairman initially binds to Owner/User.
3. CEO initially binds to Awin.
4. CIO initially binds to Pick.
5. UKS is the Unified Knowledge System managed under CIO scope.
6. Position, Role, Identity, Agent, Runtime, and System are separate concepts.
7. Position–Identity bindings are replaceable, temporal, and auditable.
8. Authority belongs primarily to Positions and Policies.
9. Runtime/provider changes must not redefine organizational identity.
10. Position-based invocation is a design target.
11. Mission is the durable business work unit.
12. Delegation should be explicit and auditable.
13. Execution should return artifacts and evidence.
14. Review should evaluate acceptance criteria rather than trust self-report.
15. Important unresolved conditions should escalate rather than be guessed through.
16. OpenSpec/Git/tests/artifacts support cross-agent continuation.
17. UKS implementation begins at Change 004, not Change 002.

## 13. Current open questions

These do not block the foundational roadmap:

- exact Department model and hierarchy rules;
- exact approval policy schema;
- Mission state machine details;
- runtime capability scoring and provider fallback policy;
- UKS connector/index technology choices;
- Control Center implementation stack;
- cost/quota governance thresholds.

They should be resolved in the Change where they belong rather than prematurely coupling layers.

## 14. Handoff to OpenSpec

This discussion explains **why** the design exists. It is not the final executable specification.

Canonical precedence:

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
This discussion
```

The immediate next OpenSpec planning target is **Change 001 — AI Company Organization Foundation**. After that, proceed to **Change 002 — Mission & Company Orchestration**.

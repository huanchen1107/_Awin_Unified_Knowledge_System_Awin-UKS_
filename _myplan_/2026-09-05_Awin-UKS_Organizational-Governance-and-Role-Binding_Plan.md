# Awin-UKS Organizational Governance and Role-Binding Plan

**Date:** 2026-09-05  
**Repository:** `huanchen1107/_Awin_Unified_Knowledge_System_Awin-UKS_`  
**Planning Source:** ChatGPT architecture discussion  
**Status:** Initial architecture plan  

---

## 1. Vision

Awin-UKS is designed as an AI-driven organizational operating model rather than only a search or RAG application.

The system separates:

- organizational positions,
- named identities,
- AI agents,
- runtime/model providers,
- systems and platforms,
- authority and delegation,
- and historical bindings between them.

The key principle is:

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

This separation allows people, AI agents, models, and tools to be replaced without changing the organizational structure or workflow.

---

## 2. Initial Executive Organization

Initial binding:

```text
Chairman / 董事長
└── Chairman Identity: User / Owner

CEO / 執行長
└── Identity: Awin

CIO / 資訊長
└── Identity: UKS
```

The initial command chain is:

```text
Chairman
   │
   ▼
CEO — Awin
   │
   ▼
CIO — UKS
```

The Chairman determines strategic intent.

Awin, as CEO, interprets Chairman intent, coordinates executive roles, delegates work, resolves cross-department responsibilities, and returns an executive-level result.

UKS, as CIO, is responsible for information governance, retrieval, knowledge routing, indexing, provenance, citations, source authority, and access to knowledge systems.

---

## 3. Important Naming Separation

`UKS` is currently bound as the **CIO identity**.

To avoid ambiguity between the CIO identity and the underlying software platform, the technical information platform should use a separate internal name.

Recommended internal platform name:

```text
UKP = Unified Knowledge Platform
```

Therefore:

```text
UKS = CIO Identity
UKP = Knowledge Platform / Infrastructure
```

Example:

```text
CIO — UKS
   │
   └── manages
       │
       ▼
Unified Knowledge Platform (UKP)
       ├── GitHub
       ├── Notion
       ├── Google Drive
       ├── NotebookLM
       ├── Search Index
       ├── Metadata Registry
       ├── Citation / Provenance
       └── MCP / Tool Gateway
```

This naming may be changed later through configuration without modifying the core architecture.

---

## 4. Core Domain Model

The system shall model the following entities independently.

### 4.1 Organization

Represents the company or organizational container.

Possible attributes:

```yaml
organization:
  id: awin-org
  name: Awin Organization
  owner_position: chairman
```

---

### 4.2 Position

A Position is an organizational office that exists independently from whoever currently occupies it.

Examples:

- Chairman
- CEO
- CIO
- CTO
- CFO
- COO
- CSO
- CRO
- Chief of Staff
- Engineering Director
- Research Director
- Security Director

Example:

```yaml
position:
  id: cio
  title: Chief Information Officer
  display_name_zh: 資訊長
  rank: 80
  reports_to: ceo
```

A Position defines organizational authority and reporting structure.

It MUST NOT directly encode a specific human, AI model, or vendor.

---

### 4.3 Role

A Role defines a reusable responsibility or capability profile.

Examples:

```text
Knowledge Governance
Executive Coordination
Software Architecture
Implementation
Code Review
Security Review
Research
Teaching
Document Management
```

A position may have multiple roles.

Example:

```yaml
position_roles:
  cio:
    - knowledge_governance
    - information_architecture
    - retrieval_management
    - source_authority_management
```

---

## 5. Identity Registry

Identity represents *who or what* is occupying a position or executing a role.

Identity types may include:

```text
Human
AI Agent
Service
Automation
Team
External Provider
```

Example:

```yaml
identities:
  chairman_owner:
    type: human
    display_name: Chairman

  awin:
    type: ai_agent
    display_name: Awin

  uks:
    type: ai_agent
    display_name: UKS
```

An identity is independent from a position.

Therefore Awin does not inherently equal CEO.

Instead:

```text
CEO Position
    │
    └── Binding
            │
            ▼
          Awin
```

---

## 6. Position–Identity Binding

Bindings determine who currently occupies each position.

Initial binding:

```yaml
bindings:
  - position_id: chairman
    identity_id: chairman_owner
    effective_from: 2026-09-05

  - position_id: ceo
    identity_id: awin
    effective_from: 2026-09-05

  - position_id: cio
    identity_id: uks
    effective_from: 2026-09-05
```

This makes bindings replaceable.

For example, in the future:

```text
CEO → Alice
Chief of Staff → Awin
CIO → Atlas
```

No organizational workflow needs to be rewritten.

Only the binding changes.

---

## 7. Historical Binding

Bindings MUST be temporal and auditable.

Do not overwrite old bindings.

Recommended structure:

```yaml
binding:
  id: binding-0003
  position_id: cio
  identity_id: uks
  effective_from: 2026-09-05T00:00:00+08:00
  effective_to: null
  status: active
  changed_by: chairman
```

When replaced:

```text
old binding → closed
new binding → active
```

This makes it possible to answer questions such as:

> Who was CIO when this decision was made?

or:

> Which AI agent generated this artifact under which organizational authority?

---

## 8. Runtime Binding

AI identity and AI runtime must also be separated.

Example:

```text
Identity: UKS
Position: CIO
Runtime: Claude
```

Later:

```text
Identity: UKS
Position: CIO
Runtime: Gemini
```

UKS remains the same organizational identity even when the underlying LLM changes.

Recommended model:

```yaml
agent_runtime_binding:
  identity_id: uks
  provider: anthropic
  model: claude
  status: active
```

Possible providers:

```text
OpenAI
Gemini
Claude
Local LLM
Specialized Agent
```

This is important for token/quota resilience.

---

## 9. Provider Failover

A runtime provider must not be treated as a permanent organizational identity.

Example:

```text
Builder Role
   │
   ├── preferred runtime → Codex
   ├── fallback → Claude
   └── fallback → Gemini
```

If Codex quota is exhausted:

```text
Role remains Builder
Position remains unchanged
Task context remains unchanged
Runtime binding changes
```

The replacement agent resumes from durable artifacts rather than previous chat history.

Recommended durable handoff sources:

- OpenSpec artifacts
- Git repository state
- design.md
- AGENTS.md
- tasks.md
- test results
- generated artifacts
- audit/event log

---

## 10. Authority Model

Authority belongs primarily to Positions and Policies, not model providers.

Example CEO authority:

```yaml
position: ceo

responsibilities:
  - interpret_chairman_intent
  - coordinate_executives
  - delegate_tasks
  - resolve_cross_department_conflicts
  - synthesize_executive_results

authority:
  can_delegate: true
  can_query_executives: true
  can_modify_governance: false
  governance_change_requires: chairman
```

Example CIO authority:

```yaml
position: cio

responsibilities:
  - knowledge_governance
  - information_retrieval
  - knowledge_index_management
  - metadata_management
  - source_provenance
  - citation_management
  - source_authority_resolution

authority:
  allowed_systems:
    - github
    - notion
    - google_drive
    - notebooklm
    - unified_knowledge_platform
```

---

## 11. Delegation Model

Delegation should follow Position → Position whenever possible.

Example:

```text
Chairman
   │ strategic instruction
   ▼
CEO — Awin
   │ delegated information task
   ▼
CIO — UKS
   │
   ├── GitHub Search
   ├── Notion Search
   ├── Google Drive Search
   ├── NotebookLM Research
   └── Knowledge Index
```

The CIO returns evidence and information analysis to the CEO.

The CEO produces the final executive response for the Chairman.

---

## 12. Position-Based Invocation

Users should be allowed to issue commands using titles instead of names.

Example:

> 叫資訊長查一下 Lesson 3 的所有資料。

Resolution:

```text
"資訊長"
   ↓
Position Registry
   ↓
CIO
   ↓
Binding Registry
   ↓
UKS
   ↓
Invoke UKS Agent
```

Similarly:

> 叫技術長處理這個 bug。

may resolve to:

```text
CTO Position
   ↓
Current Identity Binding
   ↓
Current Runtime
```

No hard-coded agent name should be required.

---

## 13. Proposed Future Executive Structure

The architecture should allow the organization to grow into:

```text
Chairman
│
└── CEO — Awin
     │
     ├── CIO — UKS
     │    ├── Knowledge Search Manager
     │    ├── Archive Manager
     │    ├── Metadata Manager
     │    └── Research Information Manager
     │
     ├── CTO — future binding
     │    ├── Architect
     │    ├── Builder
     │    ├── Reviewer
     │    └── DevOps
     │
     ├── CSO — future binding
     │    ├── Security Architect
     │    ├── Red Team
     │    └── Blue Team
     │
     ├── CRO — future binding
     │    ├── Research Agent
     │    └── Teaching Agent
     │
     └── Chief of Staff — future binding
```

The names and agents occupying these positions remain configurable.

---

## 14. UKS / CIO Responsibilities

As initial CIO, UKS should manage the future Unified Knowledge Platform.

Primary source systems:

```text
GitHub
Notion
Google Drive
NotebookLM
```

Potential future sources:

```text
Gmail
Google Calendar
Slack
Local Files
Web Sources
Databases
Vector Stores
PDF Libraries
Research Repositories
```

UKS should provide:

- source discovery,
- connector management,
- project recognition,
- metadata normalization,
- hybrid retrieval,
- semantic retrieval,
- full-text retrieval,
- source ranking,
- freshness ranking,
- authority ranking,
- provenance,
- citations,
- conflict detection,
- duplicate detection,
- deep research routing.

---

## 15. Source Authority Engine

Not all sources should be trusted equally.

An Authority Engine should determine which evidence takes precedence.

Initial conceptual priorities may look like:

```text
Canonical GitHub design/spec          100
Canonical source code                 95
Official project documentation        90
Official Google Drive document        80
Final Notion documentation            80
NotebookLM source-backed synthesis    60
Working note                          40
AI-generated summary                  20
```

These values are policy examples, not immutable constants.

The ranking should consider:

- project,
- artifact type,
- canonical status,
- source type,
- freshness,
- author,
- version,
- approval status,
- superseding relationships.

---

## 16. Knowledge Query Flow

Example Chairman request:

> Awin，找出 AI Cybersecurity Lesson 3 的所有內容並整理目前狀態。

Proposed flow:

```text
Chairman
   ↓
CEO Awin
   ↓
Intent Classification
   ↓
Delegate information request
   ↓
CIO UKS
   ↓
Knowledge Router
   ├── GitHub
   ├── Notion
   ├── Drive
   └── NotebookLM
   ↓
Authority + Freshness + Provenance
   ↓
Evidence Package
   ↓
CEO Awin
   ↓
Executive Synthesis
   ↓
Chairman
```

---

## 17. Recommended Registry Architecture

```text
organization/
├── positions/
│   ├── chairman.yaml
│   ├── ceo.yaml
│   └── cio.yaml
│
├── roles/
│   ├── executive_coordination.yaml
│   └── knowledge_governance.yaml
│
├── identities/
│   ├── chairman.yaml
│   ├── awin.yaml
│   └── uks.yaml
│
├── bindings/
│   ├── position_bindings.yaml
│   └── runtime_bindings.yaml
│
├── policies/
│   ├── authority.yaml
│   ├── delegation.yaml
│   └── source_authority.yaml
│
└── history/
    └── binding_events.jsonl
```

A database-backed registry may later replace or supplement these files.

Configuration files remain valuable as bootstrap and human-readable governance contracts.

---

## 18. Core Technical Principle

The following six concepts must remain separate:

```text
POSITION
What organizational office exists?

ROLE
What responsibility/capability is required?

IDENTITY
Who or what currently represents the actor?

AGENT
What autonomous software component performs actions?

RUNTIME
Which model/provider currently powers that agent?

SYSTEM
Which technical platform/resource is being operated?
```

Example:

```text
Position: CIO
Role: Knowledge Governance
Identity: UKS
Agent: UKS Executive Agent
Runtime: Claude / Gemini / GPT / Local
System: Unified Knowledge Platform
```

---

## 19. Auditability

Every meaningful delegated action should eventually be traceable.

Recommended event record:

```yaml
event_id: evt-...
timestamp: ...
organization_id: awin-org
initiator_position: chairman
initiator_identity: chairman_owner
delegated_to_position: ceo
delegated_to_identity: awin
subdelegated_to_position: cio
subdelegated_to_identity: uks
action: knowledge_query
systems_used:
  - github
  - google_drive
result_artifact: ...
```

This supports:

- accountability,
- reproducibility,
- debugging,
- agent handoff,
- historical reconstruction,
- future governance controls.

---

## 20. OpenSpec Roadmap

Recommended sequence:

### Change 001 — Organizational Governance and Role Binding

```text
001-organizational-governance-and-role-binding
```

Define:

- Organization Registry
- Position Registry
- Role Registry
- Identity Registry
- Position–Identity Binding
- Runtime Binding
- Delegation Policy
- Authority Policy
- Historical Binding
- Audit Event Model

This change should establish the governance foundation before implementing RAG or connectors.

### Change 002 — Unified Knowledge Connectors

Connect:

- GitHub
- Notion
- Google Drive
- NotebookLM

### Change 003 — Knowledge Catalog and Hybrid Search

Implement:

- metadata registry,
- document catalog,
- keyword index,
- vector index,
- source normalization,
- project classification.

### Change 004 — Knowledge Query Router

Implement:

- intent routing,
- project routing,
- source routing,
- authority-aware retrieval,
- freshness-aware retrieval.

### Change 005 — NotebookLM Research Layer

Use NotebookLM as deep-reader / research capability rather than canonical storage.

### Change 006 — Awin Executive Orchestration

Implement the Chairman → CEO → Executive delegation workflow.

---

## 21. Initial Decisions

The following architecture decisions are accepted as the current planning baseline:

1. The user is initially bound to the Chairman position.
2. Awin is initially bound to the CEO position.
3. UKS is initially bound to the CIO position.
4. These bindings are configurable and replaceable.
5. Organizational positions must not depend on a specific AI provider.
6. AI identities must not depend on a specific LLM runtime.
7. Runtime/provider failover must preserve identity and role continuity.
8. Historical bindings must be retained.
9. Authority belongs to organizational positions/policies, not vendors/models.
10. Delegation should primarily be position-based.
11. The CIO manages knowledge infrastructure and knowledge governance.
12. GitHub, Notion, Google Drive, and NotebookLM remain independent sources rather than mutually overwriting stores.
13. Canonical source authority and provenance must be explicitly modeled.
14. Durable project artifacts, not chat history, should enable agent handoff.
15. The knowledge platform must be distinct from the UKS CIO identity at the implementation level.

---

## 22. Desired End State

The system should eventually allow the Chairman to communicate naturally at the organizational level:

```text
「Awin，請資訊長把 Lesson 3 的資料找齊。」

「Awin，請技術長接手 Change 021。」

「資訊長，這份說明的 canonical source 在哪裡？」

「把 Builder 從 Codex 換成 Claude，繼續原本工作。」
```

The platform resolves titles, identities, runtime providers, permissions, systems, evidence, and delegation automatically.

The long-term objective is not merely an AI chatbot.

It is an **AI-governed organizational knowledge and execution system** with durable roles, replaceable agents, explicit authority, traceable delegation, and unified knowledge access.

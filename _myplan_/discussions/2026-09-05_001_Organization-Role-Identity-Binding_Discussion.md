# Change 001 Planning Discussion — Organization, Role, Identity & Binding

**Date:** 2026-09-05  
**Repository:** `huanchen1107/_Awin_Unified_Knowledge_System_Awin-UKS_`  
**Discussion Source:** Chairman + ChatGPT architecture planning  
**Purpose:** Preserve the reasoning, alternatives, accepted decisions, and scope boundary that lead into OpenSpec Change 001.

---

## 1. Background

The project began from the need for an integrated information environment spanning sources such as GitHub, Notion, Google Drive, and NotebookLM. During planning, the concept evolved from a simple unified search/RAG system into an organizational model in which AI identities occupy company positions and delegate work according to governance rules.

The Chairman established the initial executive concept:

```text
Chairman / 董事長 → User / Owner
CEO / 執行長      → Awin
CIO / 資訊長      → UKS
```

A key requirement is that company titles and names must be bindable rather than hard-coded. A position can remain stable while the person, AI identity, agent, or runtime occupying/serving it changes.

---

## 2. Central Design Question

The initial idea could have been modeled simply as:

```text
Awin = CEO
UKS = CIO
```

This was rejected as the core domain model because it permanently couples organizational offices to names.

Instead, the accepted model is:

```text
Position ── Binding ──> Identity
```

For example:

```text
CEO Position ── current binding ──> Awin Identity
CIO Position ── current binding ──> UKS Identity
```

This means Awin does not inherently equal CEO, and UKS does not inherently equal CIO. They are the initial bindings.

---

## 3. Accepted Core Principle

The most important architecture rule from this discussion is:

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

These concepts must remain independently modeled.

### Position
An organizational office, such as Chairman, CEO, CIO, CTO, CSO, or Chief of Staff.

### Role
A reusable responsibility/capability profile, such as Executive Coordination, Knowledge Governance, Architecture, Builder, Reviewer, Research, or Security Review.

### Identity
The named actor represented within the organization. It may be a human, AI identity, service, team, or automation.

### Agent
The autonomous software component capable of executing work for an identity or role.

### Runtime
The underlying AI/model/provider used to execute an agent, such as OpenAI/Codex, Claude, Gemini, or a local model.

### System
A technical resource or platform operated by the organization. Systems are not organizational positions or people.

---

## 4. Initial Organization Binding

Accepted initial binding:

```text
Chairman
└── Identity: User / Owner

CEO
└── Identity: Awin

CIO
└── Identity: UKS
```

Initial reporting chain:

```text
Chairman
   │
   ▼
CEO — Awin
   │
   ▼
CIO — UKS
```

This is an initial configuration, not an immutable architecture rule.

Future examples may include:

```text
CEO → another identity
Chief of Staff → Awin
CIO → another identity
CTO → another AI or human identity
```

The organization and workflows should continue to operate after rebinding.

---

## 5. Why Binding Is Required

Binding provides several long-term benefits.

### 5.1 Organizational stability

Company positions and reporting relationships do not need to change when personnel or AI identities change.

### 5.2 AI provider independence

Claude, Gemini, Codex, GPT, and local models must not become organizational identities merely because they execute a task.

### 5.3 Failover and quota resilience

If a runtime becomes unavailable or its quota is exhausted, the runtime can change without changing the role, identity, position, task, or organizational responsibility.

### 5.4 Historical reconstruction

Temporal bindings make it possible to answer questions such as:

- Who was CIO when a decision was made?
- Which identity occupied a position when an artifact was generated?
- Which runtime executed the work?

### 5.5 Natural-language organizational commands

The Chairman should be able to issue instructions by title, for example:

> 叫資訊長查一下。

The system can resolve:

```text
資訊長
  ↓
CIO Position
  ↓
Current Position–Identity Binding
  ↓
UKS
```

No hard-coded name is required in the Chairman's instruction.

---

## 6. Delegation Concept

The organization should prefer delegation through positions rather than direct model/provider invocation.

Conceptually:

```text
Chairman
   │ strategic intent
   ▼
CEO — Awin
   │ executive delegation
   ▼
CIO — UKS
   │
   └── information responsibilities
```

Awin's initial CEO responsibility is to understand Chairman intent, determine organizational ownership, delegate appropriately, coordinate cross-role work, and synthesize the executive result.

UKS's initial CIO responsibility is information and knowledge governance.

The detailed UKS technical implementation is explicitly outside Change 001.

---

## 7. Runtime Separation

A named AI identity must survive a runtime change.

Example:

```text
Position: CIO
Identity: UKS
Runtime: Claude
```

may later become:

```text
Position: CIO
Identity: UKS
Runtime: Gemini
```

UKS remains UKS.

Likewise, a Builder role may use:

```text
Preferred runtime → Codex
Fallback runtime  → Claude
Fallback runtime  → Gemini
```

Runtime failover must not redefine the organizational structure.

Durable handoff should rely on repository artifacts, specifications, task state, tests, audit/events, and other persisted evidence rather than depending on previous chat context.

---

## 8. Authority and Governance

Authority should belong primarily to organizational Positions and Policies rather than AI vendors/models.

Examples:

- Chairman owns strategic/governance authority.
- CEO can interpret intent and delegate executive work within policy.
- CIO can exercise information-governance authority within its assigned scope.
- Changing fundamental governance may require Chairman authority.

A runtime such as Claude or Codex receives only the authority of the identity/role/position context in which it is executing. It does not acquire authority merely because it is technically capable of an action.

---

## 9. Naming Discussion: UKS Identity vs Knowledge System

An ambiguity was identified because `UKS` is currently intended as the CIO identity while the project itself concerns a Unified Knowledge System.

The architecture therefore must distinguish:

```text
UKS as organizational identity
versus
underlying knowledge platform/system
```

A provisional internal name such as `UKP — Unified Knowledge Platform` may be used for the technical platform if necessary.

However, final naming of the UKS technical system is deliberately deferred to Change 002.

Change 001 must not solve this by coupling the CIO identity to a specific implementation.

---

## 10. Critical Scope Decision

A major decision from the discussion is that Change 001 and Change 002 must have a strict boundary.

### Change 001 — Organization Foundation

Change 001 answers questions such as:

```text
Who exists in the organization?
What positions exist?
Who currently occupies each position?
What roles/responsibilities belong to a position?
Who reports to whom?
Who may delegate to whom?
What authority does a position have?
How are bindings changed and audited?
```

Change 001 includes only organizational concepts such as:

- Organization
- Position
- Role
- Identity
- Position–Identity Binding
- reporting structure
- delegation
- authority/policy
- temporal/historical binding
- organizational audit/event model
- optional runtime-binding abstraction where necessary to preserve identity/runtime separation

### Explicitly Out of Scope for Change 001

Change 001 must NOT implement:

- GitHub knowledge connector
- Notion connector
- Google Drive connector
- NotebookLM integration
- RAG
- embeddings
- vector database
- hybrid search
- document ingestion
- document chunking
- knowledge query router
- source authority ranking for retrieved documents
- knowledge catalog
- knowledge indexing
- NotebookLM research workflow
- cross-source retrieval

The decision rule is:

> **If the question is “Who is responsible / who has authority / who is bound to the position?” it belongs to Change 001.**

---

## 11. Change 002 Boundary

Change 002 begins the actual UKS information-system work.

It answers questions such as:

```text
Where is information stored?
How does UKS connect to it?
What is a knowledge source?
What is the canonical document/source model?
How is provenance represented?
How do connectors expose source material safely and consistently?
```

Initial target sources are expected to include:

- GitHub
- Notion
- Google Drive
- NotebookLM

The detailed design belongs to Change 002 and later changes, not Change 001.

The decision rule is:

> **If the question is “Where is the information / how do we connect, retrieve, index, or integrate it?” it belongs to Change 002 or later.**

---

## 12. Proposed Roadmap After Scope Correction

```text
001 — Organization Foundation
      Organization / Position / Role / Identity / Binding /
      Reporting / Delegation / Authority / History / Audit

002 — UKS System Foundation
      Knowledge-system boundaries / source abstractions /
      connectors / canonical source-document model / provenance

003 — Knowledge Catalog & Index
      Metadata / indexing / full-text / vector / normalization

004 — Unified Retrieval & Query Router
      Intent / project / source / authority / freshness routing

005 — NotebookLM Research Layer
      Deep-reader / source-backed research capability

006 — Awin Executive Orchestration
      Chairman → CEO → executives delegation and synthesis
```

This roadmap may evolve through future planning, but Change 001's organization-only boundary is an accepted design decision.

---

## 13. Future Organizational Expansion

The binding model should permit future positions without redesigning the foundation.

Example conceptual structure:

```text
Chairman
│
└── CEO — Awin
     │
     ├── CIO — UKS
     ├── CTO — future binding
     ├── CSO — future binding
     ├── CRO — future binding
     └── Chief of Staff — future binding
```

Additional subordinate roles may later include Architect, Builder, Reviewer, Research Agent, Teaching Agent, Red Team, Blue Team, Archive Manager, and other specialist roles.

These examples are extensibility targets, not requirements to implement all positions in Change 001.

---

## 14. Accepted Decisions

The following decisions are accepted as the planning baseline for Change 001:

1. The project will use an organizational governance model rather than hard-coded agent names.
2. The user/owner is initially bound to Chairman.
3. Awin is initially bound to CEO.
4. UKS is initially bound to CIO.
5. These are bindings and can change later.
6. Position, Role, Identity, Agent, Runtime, and System are separate concepts.
7. Organizational authority belongs to positions/policies rather than model vendors.
8. Runtime/provider replacement must not change organizational identity.
9. Bindings should be temporal and auditable rather than destructively overwritten.
10. Position-based invocation should be supported conceptually.
11. Delegation should normally follow organizational positions.
12. Change 001 is strictly Organization Foundation.
13. Change 001 does not implement the UKS knowledge system.
14. Change 002 begins UKS System Foundation.
15. GitHub, Notion, Google Drive, NotebookLM, retrieval, RAG, indexing, and related knowledge-system concerns belong to Change 002 or later.

---

## 15. Open Questions Deferred

The following should not block Change 001 and can be resolved later:

- Final technical platform name when `UKS` is also the CIO identity.
- Exact GitHub/Notion/Drive/NotebookLM connector technologies.
- Whether the knowledge index is self-hosted or managed.
- Vector database selection.
- Search/reranking implementation.
- NotebookLM MCP/API implementation details.
- Exact authority ranking between knowledge sources.
- UI for the future organization chart and knowledge search system.
- Runtime/provider selection and failover implementation details beyond the abstraction needed by organizational identity.

---

## 16. Handoff to OpenSpec Change 001

This discussion is a planning source for the future OpenSpec change tentatively named:

```text
001-organizational-foundation
```

The OpenSpec artifacts should derive requirements from the accepted decisions above rather than reintroducing UKS technical-system scope.

Recommended OpenSpec artifacts:

```text
proposal.md
  Why the organization/binding foundation is needed

design.md
  Organization domain model, registries, binding lifecycle,
  delegation, authority, temporal history, audit semantics

specs/
  Behavioral requirements and invariants

tasks.md
  Incremental implementation and validation tasks
```

The implementation should remain minimal and extensible. It should establish stable contracts that Change 002 and later systems can depend on.

---

## 17. Planning Principle for Future Agents

Future Antigravity, Codex, Claude, Gemini, or other agents reading this repository should treat this document as **design rationale**, not as executable specification.

Priority should be:

```text
Accepted OpenSpec / canonical specifications
        ↓
Canonical design contracts
        ↓
Current implementation + tests
        ↓
Architecture decisions
        ↓
This planning discussion
        ↓
Historical conversation context
```

If a future canonical specification intentionally supersedes this discussion, the newer canonical decision wins, and the superseding relationship should be documented.

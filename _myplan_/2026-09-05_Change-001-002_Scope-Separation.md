# Awin-UKS Change 001 / Change 002 Scope Separation

**Date:** 2026-09-05  
**Repository:** `huanchen1107/_Awin_Unified_Knowledge_System_Awin-UKS_`  
**Status:** Accepted planning decision  

## Core Decision

Change 001 and Change 002 MUST have a strict architectural boundary.

- **Change 001 = Organization only**
- **Change 002 = UKS system begins**

The purpose is to prevent organizational governance from being mixed with knowledge-system implementation.

---

# Change 001 — Organizational Foundation

Recommended change name:

```text
001-organizational-foundation
```

## Goal

Define the company/organization model, executive structure, roles, identities, bindings, authority, delegation, and auditability.

Change 001 establishes **who exists, who reports to whom, who currently occupies each position, and what authority each position has**.

It does **NOT** implement the Unified Knowledge System.

## Initial Organization

```text
Chairman / 董事長
└── current binding: Owner / User

CEO / 執行長
└── current binding: Awin

CIO / 資訊長
└── current binding: UKS
```

Important principle:

```text
Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System
```

## Change 001 Scope

Change 001 SHOULD define:

- Organization Registry
- Position Registry
- Role Registry
- Identity Registry
- Position ↔ Identity Binding
- Reporting Structure
- Delegation Policy
- Authority Policy
- Binding History
- Organizational Audit Events
- Optional Agent Identity metadata
- Optional Runtime Binding abstraction only if needed to describe identity portability

## Change 001 Must NOT Implement

The following are explicitly out of scope:

- GitHub connector
- Notion connector
- Google Drive connector
- NotebookLM connector
- document ingestion
- RAG
- vector database
- embeddings
- hybrid search
- keyword search
- metadata extraction from knowledge sources
- knowledge catalog
- source authority ranking
- citation/provenance engine
- query router for knowledge retrieval
- NotebookLM research workflow
- UKS search UI
- MCP knowledge gateway

These belong to Change 002 or later changes.

## Change 001 Deliverable Concept

After Change 001, the system should be able to answer organizational questions such as:

```text
Who is the CEO?
→ Awin

Who is currently bound to CIO?
→ UKS

Who does CIO report to?
→ CEO

Can CIO delegate to a subordinate role?
→ Resolve through delegation policy

Change CEO from Awin to another identity.
→ Update binding without changing the position definition
```

But it should NOT yet be able to answer:

```text
Search my Google Drive.
Find Change 030 in GitHub.
Compare Notion and NotebookLM.
```

Those capabilities begin in Change 002.

---

# Change 002 — UKS System Foundation

Recommended change name:

```text
002-uks-system-foundation
```

## Goal

Begin implementation of the **Unified Knowledge System (UKS system domain)** managed under the CIO organization responsibility.

Change 002 introduces the technical knowledge-system architecture for unified information access.

## Organizational Relationship

```text
Chairman
   ↓
CEO — Awin
   ↓
CIO — UKS
   ↓
Unified Knowledge System
```

Important distinction:

- `CIO` is a **Position**.
- `UKS` is initially the **Identity bound to CIO**.
- The Unified Knowledge System is a **System** managed by the CIO domain.

The implementation MUST preserve these distinctions even if display names overlap.

## Change 002 Initial System Scope

Change 002 should start defining:

- UKS system boundary
- source connector interface
- knowledge source registry
- connector lifecycle
- authentication abstraction
- sync / live-query strategy
- document/source identity
- source metadata normalization
- provenance baseline
- project/source classification
- system-level configuration

Initial target sources:

```text
GitHub
Notion
Google Drive
NotebookLM
```

## Recommended Principle

Do not build four independent integrations with four different internal models.

Use one canonical source contract:

```text
External Source
     ↓
Connector Adapter
     ↓
Canonical UKS Source / Document Model
     ↓
Catalog / Retrieval / Research layers
```

## Change 002 Should Establish Contracts, Not Everything

Change 002 should establish the **foundation and connector contracts**.

Full search, RAG, ranking, and research orchestration should remain separate changes where practical.

---

# Revised Roadmap

## Change 001 — Organizational Foundation

```text
Organization
Positions
Roles
Identities
Bindings
Reporting
Delegation
Authority
History
Audit
```

No UKS retrieval implementation.

## Change 002 — UKS System Foundation

```text
UKS system boundary
Knowledge source registry
Connector architecture
GitHub
Notion
Google Drive
NotebookLM
Canonical source/document model
Provenance baseline
```

## Change 003 — Knowledge Catalog and Index

```text
Metadata catalog
Document registry
Chunking policy
Keyword index
Vector index
Embedding abstraction
Deduplication
Version/freshness tracking
```

## Change 004 — Unified Retrieval and Query Router

```text
Intent routing
Project routing
Source routing
Hybrid retrieval
Authority-aware ranking
Freshness-aware ranking
Conflict handling
Citation assembly
```

## Change 005 — NotebookLM Research Layer

```text
Deep reading
Cross-document research
Research delegation
Source-backed synthesis
Notebook lifecycle integration
```

NotebookLM should be treated primarily as a research/deep-reader capability, not the canonical knowledge store.

## Change 006 — Awin Executive Orchestration

```text
Chairman request
→ CEO Awin
→ organizational delegation
→ CIO UKS / future executives
→ evidence/result package
→ CEO synthesis
→ Chairman
```

---

# Boundary Rule

A simple rule should be used when reviewing future proposals:

> **If the feature answers “Who has responsibility or authority?” it belongs to Organization / Change 001.**

> **If the feature answers “Where is the information and how do we retrieve/manage it?” it belongs to UKS / Change 002 or later.**

Examples:

| Question | Change |
|---|---|
| Who is CEO? | 001 |
| Who is bound to CIO? | 001 |
| Can CEO delegate to CIO? | 001 |
| Replace Awin as CEO | 001 |
| Connect GitHub | 002 |
| Connect Notion | 002 |
| Connect Google Drive | 002 |
| Connect NotebookLM | 002 |
| Build vector search | 003 |
| Route a query across sources | 004 |
| Deep research with NotebookLM | 005 |
| CEO coordinates multiple executives | 006 |

---

# Accepted Initial Binding

```yaml
organization:
  chairman:
    position: Chairman
    current_identity: owner

  ceo:
    position: CEO
    current_identity: awin

  cio:
    position: CIO
    current_identity: uks
```

These are **bindings**, not hard-coded equivalences.

Future changes can replace identities without rewriting the organization model.

---

# Planning Conclusion

The project should proceed in two distinct phases:

```text
CHANGE 001
Build the company first.

CHANGE 002+
Build the information system operated by the company.
```

This boundary is the baseline for subsequent OpenSpec proposals and implementation planning.

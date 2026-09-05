# Awin-UKS

**Awin** is the personal AI butler / orchestrator.  
**UKS** is the **Unified Knowledge System** managed by Awin.

Awin-UKS is designed to provide one intelligent query layer across multiple personal knowledge sources without forcing every source to be copied into one application.

## Core idea

```text
User
  |
  v
Awin (AI Butler / Orchestrator)
  |
  +--> Query Router
  |      +--> Live Source Search
  |      +--> Unified Retrieval / RAG
  |      +--> Deep Research
  |
  v
UKS (Unified Knowledge System)
  |
  +--> GitHub
  +--> Notion
  +--> Google Drive
  +--> NotebookLM
```

## Roles

### Awin
Awin is the user-facing intelligence layer. It understands intent, decides where to search, combines evidence, preserves source authority, and returns cited answers.

Awin should not be the permanent store of truth. It is the **orchestrator, router, researcher, and butler** of the knowledge system.

### UKS
UKS is the knowledge infrastructure behind Awin. It provides:

- connectors to knowledge sources
- metadata normalization
- indexing and retrieval
- source authority rules
- citations and provenance
- semantic + keyword search
- project-aware retrieval
- optional NotebookLM deep-reading workflows
- MCP/API access for AI agents and coding tools

## Source-of-truth policy

The original systems remain authoritative:

- **GitHub** — code, OpenSpec changes, architecture, technical decisions
- **Notion** — notes, planning, structured knowledge, teaching notes
- **Google Drive** — PDFs, slides, documents, spreadsheets, source materials
- **NotebookLM** — deep reading, source-grounded synthesis, research workspace

UKS indexes and routes knowledge; it should avoid unnecessary bidirectional synchronization.

## Initial architecture

```text
                         +----------------------+
                         |        USER          |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         |        AWIN          |
                         | AI Butler / Router   |
                         +----------+-----------+
                                    |
                    +---------------+---------------+
                    |               |               |
                    v               v               v
             Project Router    Source Router   Research Router
                    |               |               |
                    +---------------+---------------+
                                    |
                                    v
                         +----------------------+
                         |         UKS          |
                         +----------+-----------+
                                    |
          +-------------------------+-------------------------+
          |                         |                         |
          v                         v                         v
  Live Connectors             Retrieval Index          Research Layer
          |                         |                         |
   +------+------+          +------+------+             NotebookLM
   |      |      |          |      |      |
 GitHub Notion Drive      Vector Keyword Metadata
```

## Query examples

### Technical / canonical query

> Awin, find where Change 030 defines FVG mitigation.

Expected routing:

```text
GitHub live search
 -> OpenSpec
 -> design / specs / tasks
 -> source code if needed
 -> answer with citations
```

### Cross-source query

> Awin, compare my Lesson 3 GitHub plan with my Drive PDFs and Notion notes.

Expected routing:

```text
GitHub + Google Drive + Notion
 -> normalize evidence
 -> authority ranking
 -> synthesize
 -> cite every source
```

### Deep-reading query

> Awin, read these cybersecurity PDFs and explain how they should change Lesson 3.

Expected routing:

```text
Drive sources
 -> NotebookLM / deep research layer
 -> structured findings
 -> compare with GitHub canonical course plan
 -> final recommendations
```

## Proposed repository structure

```text
_Awin_Unified_Knowledge_System_Awin-UKS_/
├── README.md
├── AGENTS.md
├── docs/
│   ├── architecture.md
│   ├── source-authority.md
│   ├── metadata-schema.md
│   └── query-routing.md
├── connectors/
│   ├── github/
│   ├── notion/
│   ├── google-drive/
│   └── notebooklm/
├── uks/
│   ├── ingestion/
│   ├── indexing/
│   ├── retrieval/
│   ├── metadata/
│   └── provenance/
├── awin/
│   ├── router/
│   ├── agents/
│   ├── policies/
│   └── prompts/
├── mcp/
│   └── awin-uks-mcp/
├── web/
└── openspec/
```

## Design principles

1. **Awin is the butler; UKS is the system.**
2. **Sources remain sources of truth.**
3. **Retrieve before copying.**
4. **Live lookup for fast-changing technical sources.**
5. **RAG for broad semantic discovery.**
6. **NotebookLM is a deep-reader, not the central database.**
7. **Every answer should preserve provenance.**
8. **Project and authority metadata matter as much as embeddings.**
9. **Prefer hybrid retrieval: metadata + keyword + vector + live tools.**
10. **The architecture must remain usable by ChatGPT, Codex, Claude, Gemini, Antigravity, and future agents through MCP/API boundaries.**

## Status

This repository is the starting point for the Awin-UKS architecture.

Next recommended milestone: **Change 001 — Awin-UKS Foundation Architecture**.

# AGENTS.md — Awin AI Company Repository Contract

This repository is operated as an AI Company project using OpenSpec for durable change execution and `_myplan_` for Chairman/ChatGPT planning history, rationale, and accepted planning inputs.

## 1. Canonical organization

```text
Chairman / 董事長 → Owner / User
CEO / 執行長      → Awin
CIO / 資訊長      → Pick
UKS               → Unified Knowledge System
```

Invariant:

> Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System

Do not reintroduce the superseded interpretations `CIO = UKS identity`, `Change 002 = UKS`, or `Awin = only a knowledge-search butler`.

## 2. Durable work model

- `_myplan_` stores planning discussions, accepted decisions, roadmap, and implementation feedback.
- `openspec/` stores executable change contracts and canonical specifications.
- Git commits, source, tests, artifacts, and review evidence prove implementation state.
- Chat history is temporary context and MUST NOT be the only source needed to resume work.
- Mission is the preferred durable business/work unit beginning in Change 002.

## 3. Mandatory planning-to-OpenSpec bridge

Before creating or modifying any OpenSpec change:

1. Read `_myplan_/planning-manifest.yaml`.
2. Read `_myplan_/change-registry.yaml`.
3. Resolve the requested change ID/name from the registry.
4. Read every planning source listed for that change that is marked required or canonical.
5. Read `_myplan_/decisions/` entries referenced by the manifest.
6. Treat accepted decisions as higher authority than historical discussion.
7. Record planning provenance in the OpenSpec change artifacts.
8. Never silently contradict an accepted planning decision.

If a requested change has no registry entry or its planning status is not ready, do not invent missing planning. Record the gap and escalate through the feedback process.

## 4. Controlled reverse flow: OpenSpec → `_myplan_`

Implementation discoveries MUST NOT silently rewrite planning history.

When execution, review, testing, or runtime constraints reveal a conflict, new fact, or architecture concern:

1. Create a feedback record under `_myplan_/feedback/`.
2. Reference the OpenSpec change, task, commit/test/evidence, and affected planning source.
3. Classify it as `finding`, `conflict`, `risk`, `proposal`, or `escalation`.
4. Do not modify an accepted decision unless a new decision explicitly supersedes it.
5. After a new decision is accepted, update the planning manifest/change registry and then update OpenSpec artifacts as required.

This is controlled bidirectional synchronization, not arbitrary two-way editing.

## 5. Planning precedence

When sources conflict, use this order unless an explicit superseding decision says otherwise:

```text
Accepted canonical OpenSpec specs
        ↓
Accepted OpenSpec design/contracts
        ↓
Verified implementation + tests/evidence
        ↓
Accepted `_myplan_/decisions/`
        ↓
Canonical integrated roadmap
        ↓
Planning discussion / historical notes
        ↓
Conversation-only context
```

During a proposed change, delta/change artifacts may intentionally supersede current specs after review and archive.

## 6. OpenSpec workflow

Use the installed OpenSpec workflow rather than manually inventing artifact layouts.

Default project schema: `spec-driven`.

Expected artifact flow:

```text
proposal → specs → design → tasks → apply → verify/review → archive
```

Before apply:
- validate scope against `_myplan_/change-registry.yaml`;
- ensure planning provenance is present;
- ensure tasks are owned by logical roles, not hard-coded model vendors.

After apply:
- attach or cite evidence;
- update task state;
- create feedback only for new findings/conflicts;
- do not claim completion without evidence.

## 7. Runtime/provider independence

Claude, Codex, Gemini, GPT, Antigravity, and future runtimes are replaceable execution providers.

A provider failure or quota exhaustion MUST NOT change:
- organizational position;
- identity binding;
- mission/change ownership;
- task intent;
- accepted planning contracts.

Resume from durable repository state, OpenSpec artifacts, Git state, tests, evidence, manifest, and registry rather than previous chat history.

## 8. Current roadmap

```text
001 — AI Company Organization Foundation
002 — Mission & Company Orchestration
003 — Agent Runtime & Execution Fabric
004 — UKS: Unified Knowledge System
005 — Awin CEO Orchestrator
006 — AI Company Control Center
```

Change boundaries are canonical in `_myplan_/change-registry.yaml`.

## 9. Safety and authority

- Prefer position-based delegation over provider/model-based delegation.
- Do not perform destructive or governance-changing actions without the required approval.
- Do not silently rewrite historical accepted decisions.
- Preserve provenance and auditability.
- Prefer minimal, evidence-backed changes over broad speculative rewrites.

## 10. Required handoff package

When handing work to another agent/runtime, leave enough durable state to continue without chat history:

- change ID and current status;
- active task(s);
- relevant `_myplan_` source refs;
- OpenSpec artifact paths;
- current Git status/commit refs where available;
- tests/evidence run;
- unresolved blockers/risks;
- next recommended action.

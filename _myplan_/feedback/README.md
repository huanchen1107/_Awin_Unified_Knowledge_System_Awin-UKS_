# `_myplan_/feedback/` — Controlled Reverse Feedback

This directory receives implementation/review findings that may affect planning.

It is the controlled reverse path from OpenSpec execution back into planning.

## Do not rewrite accepted history silently

An agent MUST NOT edit an accepted decision merely because implementation found a better approach.

Use this flow:

```text
OpenSpec / implementation / review
        ↓
new finding, conflict, risk, proposal, or escalation
        ↓
_myplan_/feedback/<record>.md
        ↓
Chairman / CEO / responsible governance review
        ↓
accepted new decision if required
        ↓
update planning manifest / change registry
        ↓
update OpenSpec artifacts
```

## Suggested filename

```text
YYYY-MM-DD_change-<id>_<short-topic>.md
```

Example:

```text
2026-09-05_change-001_binding-concurrency.md
```

## Required fields

Every feedback record should include:

```yaml
feedback_id: FB-...
date: YYYY-MM-DD
change_id: "001"
classification: finding | conflict | risk | proposal | escalation
status: open | accepted | rejected | superseded | resolved
reported_by_role: ...
source_openspec_path: ...
source_task: ...
evidence:
  - commit/test/artifact/path
planning_sources_affected:
  - ...
summary: ...
recommendation: ...
requires_decision: true | false
```

## Authority

Feedback is evidence/input, not automatically a planning decision.

If it contradicts an accepted decision, the old decision remains authoritative until an explicit new decision supersedes it.

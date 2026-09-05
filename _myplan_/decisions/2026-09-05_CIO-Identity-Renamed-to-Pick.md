# Architecture Decision — CIO Identity Renamed from UKS to Pick

**Date:** 2026-09-05  
**Status:** Accepted  
**Applies to:** Change 001 Organization Foundation and all subsequent planning

## Decision

The CIO identity previously named `UKS` is renamed to `Pick`.

The canonical initial organizational binding is now:

```text
Chairman / 董事長 → User / Owner
CEO / 執行長      → Awin
CIO / 資訊長      → Pick
```

## Canonical Naming Separation

From this decision forward:

```text
Awin = CEO Identity
Pick = CIO Identity
UKS  = Unified Knowledge System
```

`UKS` is no longer the name of the CIO identity. It is reserved for the information/knowledge system beginning with Change 002.

## Organization Relationship

```text
Chairman
   │
   ▼
CEO — Awin
   │
   ▼
CIO — Pick
   │
   ▼
UKS — Unified Knowledge System
```

The relationship remains binding-based rather than hard-coded:

```text
Position ── Binding ──> Identity
```

Initial bindings:

```yaml
bindings:
  chairman:
    identity: owner
  ceo:
    identity: awin
  cio:
    identity: pick
```

These bindings may change in the future without redefining the positions or organizational workflow.

## Scope Consequence

### Change 001 — Organization Foundation

Change 001 uses:

```text
Chairman → Owner
CEO      → Awin
CIO      → Pick
```

It remains strictly concerned with Organization, Position, Role, Identity, Binding, Reporting, Delegation, Authority, History, and Audit.

### Change 002 — UKS System Foundation

Change 002 begins design and implementation of:

```text
UKS = Unified Knowledge System
```

Pick, as the initially bound CIO identity, is organizationally responsible for UKS, but Pick and UKS are separate domain concepts:

```text
Identity: Pick
Position: CIO
System: UKS
```

This follows the established invariant:

> **Position ≠ Role ≠ Identity ≠ Agent ≠ Runtime ≠ System**

## Superseding Rule

Any earlier planning text stating `UKS = CIO`, `CIO — UKS`, or `UKS Identity` should be interpreted as historical discussion and is superseded by this decision.

Canonical interpretation after 2026-09-05:

```text
CIO — Pick
UKS — Unified Knowledge System
```

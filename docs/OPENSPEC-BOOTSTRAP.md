# OpenSpec Bootstrap — Awin AI Company

This repository contains OpenSpec project context and the Awin planning-governance bridge, but the OpenSpec CLI itself is installed on each developer/agent machine, not inside GitHub.

## Official package

Use the current Fission AI OpenSpec package:

```bash
npm install -g @fission-ai/openspec@latest
```

Check:

```bash
openspec --version
```

## First setup after clone

From the repository root, use the official OpenSpec CLI to install/refresh AI tool integrations.

Recommended for this project:

```bash
openspec init --tools antigravity,codex,claude,gemini
```

If OpenSpec already recognizes the repository and `openspec/config.yaml` exists, prefer refreshing generated tool integrations rather than replacing project governance content:

```bash
openspec update
```

Do not delete or overwrite the custom project context in `openspec/config.yaml`.

## Verify

```bash
openspec list
openspec list --specs
```

Before creating Change 001, verify the repository contains:

```text
AGENTS.md
openspec/config.yaml
openspec/specs/
openspec/changes/
_myplan_/planning-manifest.yaml
_myplan_/change-registry.yaml
_myplan_/feedback/
```

## Governance bridge

OpenSpec's `config.yaml` injects the repository's planning rules into artifact generation. Agents must additionally resolve exact Change-specific planning sources through:

```text
_myplan_/planning-manifest.yaml
_myplan_/change-registry.yaml
```

The forward path is:

```text
_myplan_ → manifest/registry → OpenSpec → implementation/evidence
```

The reverse path is controlled:

```text
implementation/review → _myplan_/feedback → accepted decision → OpenSpec update
```

## Change 001

After CLI integration is verified, the next official change is:

```text
001-ai-company-organization-foundation
```

Use the OpenSpec workflow to create artifacts. Do not manually invent a competing artifact structure.

## Upgrade on another device

After updating the package:

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

Then re-read `AGENTS.md` and the planning manifest before continuing active work.

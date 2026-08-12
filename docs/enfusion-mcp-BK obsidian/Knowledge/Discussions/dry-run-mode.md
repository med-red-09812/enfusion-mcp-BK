---
type: discussion
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
addon-version: null
engine-version: null
last-reviewed: 2026-08-12
sources:
  - UPGRADE_IDEAS.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
---

# Dry-run mode for mutation tools

## Question / scope

Should `mod_create`, `script_create`, `prefab_create`, `config_create`, `layout_create`, and `project_write` accept a `dryRun: boolean` parameter that returns what would be created/modified without writing to disk? Scope: mutation tools that immediately write via writeFileSync.

## Evidence

- UPGRADE_IDEAS.md item 10 (OPEN - Tier 1 quick win): "All creation tools immediately write to disk via writeFileSync. There's no way for Claude to preview what it's about to generate and course-correct before committing. `script_create` already has a partial pattern - when a file already exists (`src/tools/script-create.ts:78`), it returns the generated code without writing."
- Listed Effort: S; Category: UX Polish.

## Inference / proposal

Generalize the existing script_create no-write behavior into a dryRun parameter across all creation tools, so the LLM can preview generated output before committing.

## Alternatives and trade-offs

- Option: dryRun flag per tool.
  - Benefits: consistent preview capability; small effort.
  - Costs or risks: parameter surface grows across 6 tools; behavior must be documented per tool.
- Option: separate preview tool.
  - Benefits: no per-tool parameters.
  - Costs or risks: new tool registration; must mirror every mutation tool's schema.

## Open questions

- Should dryRun also report the file path and any collision warnings?
- How should dryRun interact with mod_validate in the create-mod prompt workflow?

## Verification boundary

- Verified: item 10 is open in UPGRADE_IDEAS.md (not struck through; no PR reference).
- Not verified: any implementation (no dryRun evidence found in src/tools during vault creation).
- Evidence needed next: prompt-workflow design showing where preview fits.

## Related notes

- [[Knowledge/Discussions/index|Discussions index]]

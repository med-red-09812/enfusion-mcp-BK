---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources:
  - TODO.md
  - UPGRADE_IDEAS.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
tags: [index, discussion]
---

# Discussions

Open questions and proposals from TODO.md and UPGRADE_IDEAS.md that are NOT yet decided. These are proposals, not decisions - do not cite them as approved. Status: draft.

## Topic indexes

- [[Knowledge/Discussions/recipe-pattern-system/index|Recipe / Pattern System]] - pattern-composition question.
- [[Knowledge/Discussions/prefab-scenario-tooling/index|Prefab & Scenario Tooling]] - scenario_create_objective questions.
- [[Knowledge/Discussions/workbench-integration/index|Workbench Integration]] - Workbench/API feedback and workspace questions.

## Entries

- [[Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-layer-file-write|scenario_create_objective: write hierarchy directly to .layer]] - would fix nesting persistence and the enum-set limitation (TODO.md).
- [[Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-spawn-options|scenario_create_objective: spawn radius and multiple SlotAI]] - m_sSpawnRadius offset + aiSpawnCount parameter (TODO.md FEATs).
- [[Knowledge/Discussions/workbench-integration/dry-run-mode|Dry-run mode for mutation tools]] - preview what would be written without writing (UPGRADE_IDEAS #10).
- [[Knowledge/Discussions/workbench-integration/method-signature-validator|Method signature validator tool]] - lightweight "did I get this right?" API check (UPGRADE_IDEAS #13).
- [[Knowledge/Discussions/workbench-integration/compile-error-feedback|Compilation error feedback + log capture]] - let Claude see Workbench errors instead of guessing (UPGRADE_IDEAS #16).
- [[Knowledge/Discussions/recipe-pattern-system/pattern-composition|Pattern composition (mix-and-match)]] - multiple patterns in one mod_create; status ambiguous in UPGRADE_IDEAS (item 7).
- [[Knowledge/Discussions/workbench-integration/multi-mod-workspace|Multi-mod workspace support]] - workspace model over single projectPath (UPGRADE_IDEAS #23).

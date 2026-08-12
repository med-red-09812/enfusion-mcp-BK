---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources:
  - docs/superpowers/specs/2026-03-31-prefab-recipe-system-design.md
  - docs/superpowers/specs/2026-03-25-prefab-ancestry-resolver-design.md
  - docs/superpowers/specs/2026-03-27-tool-discoverability-design.md
  - docs/plans/2026-03-19-workbench-api-expansion-design.md
  - docs/plans/2026-03-02-scenario-create-objective-design.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
---

# Decisions

Real design decisions recorded in committed design specs (docs/plans/ and docs/superpowers/specs/). The GitHub tracker has issues disabled, so these transcriptions are sourced from the spec documents themselves, which carry explicit decision tables or "Approved" status. Status: review until a maintainer confirms the transcription matches the specs.

## Entries

- [[Knowledge/Decisions/prefab-recipe-system|Prefab Recipe System]] - JSON recipes replace hardcoded PREFAB_CONFIGS; 12 categories + variants (spec Status: Approved, 2026-03-31).
- [[Knowledge/Decisions/prefab-ancestry-resolver|Prefab Ancestry Resolver]] - extract ancestry resolution into shared utility; used by game_duplicate and prefab_create (explicit Decisions table, 2026-03-25).
- [[Knowledge/Decisions/tool-consolidation|Tool Consolidation 54 -> 47]] - merge related tool families behind an action discriminator (design 2026-03-27).
- [[Knowledge/Decisions/workbench-api-expansion|Workbench API Expansion Scope]] - 8 items (1 bug fix, 6 new actions, 1 new tool) from IDA Pro findings; no breaking changes (design 2026-03-19).
- [[Knowledge/Decisions/scenario-create-objective-orchestration|scenario_create_objective as Orchestration Tool]] - pure orchestration over existing wb_entity_create/modify, no new Workbench scripts (design Status: Approved, 2026-03-02).

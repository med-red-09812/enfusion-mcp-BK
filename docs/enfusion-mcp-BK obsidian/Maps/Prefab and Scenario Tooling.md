---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources: []
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Lessons/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
tags: [moc, prefab-scenario-tooling]
---

# Prefab and Scenario Tooling

Prefab creation and Scenario Framework hierarchy building. The ancestry resolver feeds inherited components into prefab generation (including the recipe system); scenario objective placement builds Area -> LayerTask -> Slot hierarchies and its open bugs are about whether the saved .layer matches the in-memory nesting.

## Notes

- [[Knowledge/Decisions/prefab-ancestry-resolver|Prefab Ancestry Resolver (decision)]] - shared src/utils/prefab-ancestry.ts resolves delta-based inheritance for game_duplicate and prefab_create.
- [[Knowledge/Decisions/scenario-create-objective-orchestration|scenario_create_objective as Orchestration Tool (decision)]] - pure orchestration over wb_entity_create/modify, no new Workbench scripts.
- [[Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-layer-file-write|Layer-file-write (discussion)]] - generate the .layer block directly to fix nesting persistence and the enum-set limitation.
- [[Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-spawn-options|Spawn radius and multiple SlotAI (discussion)]] - m_sSpawnRadius offset + aiSpawnCount as additive parameters.
- [[Knowledge/Lessons/scenario-slot-direct-child|Slots must be direct children of LayerTask (lesson)]] - GetSlotTask only searches direct children; saved .layer nesting may not persist.

## Areas drawn from

- [[Knowledge/Decisions/index|Decisions]]
- [[Knowledge/Lessons/index|Lessons]]
- [[Knowledge/Discussions/index|Discussions]]

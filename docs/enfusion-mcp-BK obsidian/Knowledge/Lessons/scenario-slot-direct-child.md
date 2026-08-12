---
type: lesson
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
addon-version: null
engine-version: null
last-reviewed: 2026-08-12
sources:
  - TODO.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Lessons/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/scenario-create-objective-layer-file-write.md
---

# Scenario Framework slots must be direct children of LayerTask

## Situation

`scenario_create_objective` produced the error `ScenarioFramework: USSR_Ambush_LayerTask could not init task due to missing m_SlotTask!` because the SlotKill entity ended up nested inside Layer_AI instead of being a direct child of LayerTask.

## Evidence

- TODO.md root cause: `GetSlotTask(m_aChildren)` in `SCR_ScenarioFrameworkLayerTask` only searches *direct* children of LayerTask for `SCR_ScenarioFrameworkSlotTask`.
- Required hierarchy documented in TODO.md:
  - Area -> LayerTask -> Slot (SlotKill / SlotClearArea / SlotDestroy), direct child of LayerTask -> Layer_AI -> SlotAI.
- TODO.md notes the reparent order in wb-scenario.ts was fixed, but Workbench appears to rewrite the layer file with the slot inside Layer_AI anyway; investigation needed on whether `ParentEntity(false)` in EMCP_WB_ModifyEntity.c nests correctly on save.

## Inference / proposal

The scenario framework's slot lookup is hierarchy-position-sensitive, not name-based. Fixing the in-memory hierarchy is not sufficient if the saved .layer file does not persist the correct nesting.

## Lesson

When building Scenario Framework hierarchies through the Workbench NET API, ensure slots are direct children of the LayerTask node and verify the persisted .layer file, not just the in-memory hierarchy.

## Verification boundary

- Verified: root cause analysis and required hierarchy are documented in TODO.md (committed evidence).
- Not verified: whether the layer-file-write approach (proposed in TODO.md FEAT section) has been implemented; the investigation into ParentEntity(false) save behavior is still open.
- Regression coverage or follow-up: see the discussion note on layer-file-write.

## Related notes

- [[Knowledge/Discussions/scenario-create-objective-layer-file-write|scenario_create_objective layer-file-write discussion]]
- [[Knowledge/Decisions/scenario-create-objective-orchestration|scenario_create_objective orchestration decision]]

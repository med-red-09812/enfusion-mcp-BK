---
type: decision
audience: maintainer
visibility: internal
status: review
publish-targets: []
owner: enfusion-mcp-BK maintainers
addon-version: null
engine-version: null
last-reviewed: 2026-08-12
sources:
  - docs/plans/2026-03-02-scenario-create-objective-design.md
  - docs/plans/2026-03-02-scenario-create-objective.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-layer-file-write.md
tags: [decision, prefab-scenario-tooling]
---

# scenario_create_objective as Orchestration Tool

## Context

Adding a `scenario_create_objective` MCP tool that places a complete Scenario Framework objective hierarchy (Area -> LayerTask -> Layer_AI -> SlotKill + SlotAI) in a live Workbench scene in a single call. The design needed to choose between pure orchestration over existing tools vs. new Workbench-side scripts.

## Evidence

- Design `2026-03-02-scenario-create-objective-design.md` is marked "Status: Approved".
- Plan `2026-03-02-scenario-create-objective.md` states the architecture: "Pure orchestration tool - no new Workbench scripts. Calls existing wb_entity_create and wb_entity_modify (setProperty / reparent) sequentially."
- Implementation exists: `src/tools/wb-scenario.ts` registered via `registerScenarioTools` in src/server.ts; prefab GUIDs were verified from game .et files (listed in the plan).
- Later issues are tracked in TODO.md (nesting persistence and enum-setting bugs - see related discussion note).

## Inference / proposal

Reusing the existing wb_entity_* tools avoids new Workbench-side handler scripts for the common case, keeping the mod addon surface small. Entity names derive from `taskName` for deterministic cross-references.

## Decision

- Implement `scenario_create_objective` as pure orchestration: place entities via wb_entity_create, wire hierarchy via wb_entity_modify reparent/setProperty.
- No new EnforceScript handlers for this tool.
- Name all placed entities from the taskName parameter (e.g. `{taskName}_Area`, `{taskName}_LayerTask`).

## Consequences

### Benefits

- No new Workbench scripts; reuse of tested tool primitives; deterministic naming for cross-references.

### Costs and risks

- Reliance on reparent/setProperty round-tripping through the NET API: TODO.md documents that saved .layer nesting may not persist correctly and that enum string values cannot be set via setProperty - both are open problems tracked in Discussions.

## Verification boundary

- Verified: design approved; orchestration implementation exists in src/tools/wb-scenario.ts.
- Not verified: end-to-end correctness of the produced .layer hierarchy in a live Workbench save (open per TODO.md).
- Revisit trigger: resolution of the TODO.md bugs (layer-file-write approach would supersede the orchestration decision for hierarchy building).

## Related notes

- [[Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-layer-file-write|scenario_create_objective layer-file-write discussion]]

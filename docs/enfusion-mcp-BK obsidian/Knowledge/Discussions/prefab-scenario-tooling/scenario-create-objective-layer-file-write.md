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
  - TODO.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Lessons/scenario-slot-direct-child.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/scenario-create-objective-orchestration.md
tags: [discussion, prefab-scenario-tooling]
---

# scenario_create_objective: write hierarchy directly to .layer

## Question / scope

Should `scenario_create_objective` stop placing entities one-by-one via the NET API (create + reparent) and instead generate the complete `.layer` file block directly? Scope: the `scenario_create_objective` tool only; the question is about persistence of the Scenario Framework hierarchy.

## Evidence

- TODO.md [BUG]: SlotKill ends up inside Layer_AI, not as a direct LayerTask child - Workbench appears to rewrite the layer file with the wrong nesting despite the in-memory reparent fix. The in-memory reparent via `EMCP_WB_ModifyEntity` may not produce the correct saved nesting.
- TODO.md [BUG]: `m_eActivationType` ON_TRIGGER_ACTIVATION cannot be set via `setProperty` - enum string values silently fail through `SetVariableValue`; must be written directly to the .layer file.
- TODO.md [FEAT]: "Instead of placing entities one-by-one via API and reparenting (which has nesting persistence issues), generate the complete `.layer` file block and append/write it directly. This would also solve the `m_eActivationType` enum issue."
- TODO.md mentions a manual fix that worked before (writing the hierarchy directly to the layer file).

## Inference / proposal

A layer-file-write approach would solve both open bugs in one change: correct nesting in the saved file and enum values that setProperty cannot handle. Investigation is needed first: verify whether `ParentEntity(false)` in `EMCP_WB_ModifyEntity.c` nests correctly in the saved .layer, or whether Workbench flattens/reorders children on save.

## Alternatives and trade-offs

- Option: Write full hierarchy directly to .layer from the tool.
  - Benefits: solves nesting + enum issues together; matches the "manual fix that worked".
  - Costs or risks: bypasses Workbench undo/reparent; must keep the file format in sync with Workbench's writer; entities not visible in editor until reload.
- Option: Keep API orchestration, fix reparent handler.
  - Benefits: stays within tested wb_entity_* primitives.
  - Costs or risks: does not solve the enum-set limitation; persistence bug may be unfixable from the handler side.

## Open questions

- Does `ParentEntity(false)` in EMCP_WB_ModifyEntity.c produce correct saved nesting, or does Workbench reorder on save?
- Would the layer-file-write approach survive Workbench re-saving the world afterward?

## Verification boundary

- Verified: both bugs and the proposed FEAT are documented in TODO.md.
- Not verified: behavior of the save path; whether the FEAT has been implemented (no code evidence found in src/tools/wb-scenario.ts during vault creation).
- Evidence needed next: a test saving a reparented hierarchy and inspecting the .layer; comparison with the previously-working manual file write.

## Related notes

- [[Knowledge/Lessons/scenario-slot-direct-child|Scenario Framework slots must be direct children of LayerTask]]
- [[Knowledge/Decisions/scenario-create-objective-orchestration|scenario_create_objective orchestration decision]]

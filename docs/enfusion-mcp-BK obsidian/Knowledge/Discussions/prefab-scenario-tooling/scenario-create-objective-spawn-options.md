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
tags: [discussion, prefab-scenario-tooling]
---

# scenario_create_objective: spawn radius and multiple SlotAI

## Question / scope

Should `scenario_create_objective` support placing SlotAI at an offset from the area center, and should it support multiple SlotAI entities under Layer_AI? Scope: the `scenario_create_objective` tool's spawn-placement parameters.

## Evidence

- TODO.md [FEAT]: "add `m_sSpawnRadius` / spawn offset support - Allow placing SlotAI at an offset from the area center, so AI spawns slightly away from the trigger edge."
- TODO.md [FEAT]: "Support for multiple SlotAI entities under Layer_AI - Currently only one SlotAI is placed. Allow an optional `aiSpawnCount` parameter to place N SlotAIs."

## Inference / proposal

Both features are additive parameter extensions to the existing tool (spawn offset, aiSpawnCount). They are open work items, not decided scope.

## Alternatives and trade-offs

- Option: single offset parameter (m_sSpawnRadius).
  - Benefits: minimal interface change.
  - Costs or risks: does not cover fan-out of multiple AI groups.
- Option: aiSpawnCount for N SlotAIs.
  - Benefits: richer scenarios (multiple groups).
  - Costs or risks: more entities per call; interplay with the layer-file-write discussion.

## Open questions

- Should offset and aiSpawnCount be independent, or bundled into a spawn-pattern parameter?
- Does the layer-file-write approach (see related note) change how these parameters would be expressed?

## Verification boundary

- Verified: both FEATs are listed as open in TODO.md.
- Not verified: any implementation or design for them (none found in src/tools/wb-scenario.ts during vault creation).
- Evidence needed next: user scenarios that justify the parameters; interaction with the open nesting bug.

## Related notes

- [[Knowledge/Discussions/prefab-scenario-tooling/scenario-create-objective-layer-file-write|scenario_create_objective layer-file-write discussion]]

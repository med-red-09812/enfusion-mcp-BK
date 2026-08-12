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
  - docs/superpowers/specs/2026-03-31-prefab-recipe-system-design.md
  - docs/superpowers/plans/2026-03-31-prefab-recipe-system.md
  - RECIPE_SYSTEM_OVERVIEW.md
  - RECIPE_ARCHITECTURE.txt
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/prefab-ancestry-resolver.md
---

# Prefab Recipe System

## Context

`prefab create` had 7 hardcoded type configs in TypeScript (character, vehicle, weapon, spawnpoint, gamemode, interactive, generic) with 2-3 placeholder components each. Resulting prefabs were incomplete (e.g. "make a handgun" produced only WeaponComponent + MeshObject) and extending the system required code changes. The ancestry resolver already pulled inherited components from base game prefabs, but the tool did not know which base game parent to point to.

## Evidence

- Spec `2026-03-31-prefab-recipe-system-design.md` is marked "Status: Approved".
- RECIPE_SYSTEM_OVERVIEW.md documents the implemented system: 12 recipe categories, 16 variants, 28 creation paths; recipe loader (182 lines), schema (55 lines), 12 JSON files (514 lines), prefab template (142 lines), prefab tool (332 lines).
- RECIPE_ARCHITECTURE.txt documents architecture layers and the component merge strategy (ancestor GUIDs win, then recipe overrides, then user components).
- Implementation exists in the tree: `src/templates/recipe.ts`, `src/templates/recipe-loader.ts`, `data/recipes/*.json` (12 files), `src/tools/prefab.ts` with variant parameter.

## Inference / proposal

The approved approach is a hybrid: TypeScript schema for build-time validation, JSON data files editable without code changes, and a loader that validates at runtime and merges variant overrides. Recipes are deliberately thin guidance layers - they point at the correct base parent prefab and let the ancestry resolver fill in inherited components.

## Decision

- Replace hardcoded `PREFAB_CONFIGS` with JSON recipes (12 categories: firearm, attachment, ground_vehicle, air_vehicle, character, prop, building, item, group, spawnpoint, gamemode, generic).
- Add optional `variant` parameter (16 variants total, e.g. handgun/rifle/launcher/machinegun under firearm).
- Remove old type names (weapon, vehicle, interactive); `prefabType` enum expands to 12 values.
- Recipes define: defaultParent path, overrideComponents with placeholder values and guidance comments, postCreateNotes checklists.
- Tool responses include post-creation checklists formatted as `[ ] Item`.

## Consequences

### Benefits

- Extensible without code rebuild (JSON data); centralized auditable data; correct base game parent references; automatic inheritance via ancestry resolver; reduced user cognitive load.

### Costs and risks

- Runtime schema validation required (implemented); recipe paths assume standard game structure (hardcoded); no dynamic parent discovery; placeholder values are empty (user must fill model/sound paths); checklists can drift as the game evolves.

## Verification boundary

- Verified: spec marked Approved; implementation and data files present in tree; RECIPE_SYSTEM_OVERVIEW claims 17 prefab tests and 136+ template tests passing.
- Not verified: test runs were not re-executed during vault creation; "production-ready" claim in the overview not independently confirmed.
- Revisit trigger: any change to recipe file structure, variant semantics, or prefabType enum.

## Related notes

- [[Knowledge/Decisions/prefab-ancestry-resolver|Prefab Ancestry Resolver]] - the resolver is the dependency this system relies on.

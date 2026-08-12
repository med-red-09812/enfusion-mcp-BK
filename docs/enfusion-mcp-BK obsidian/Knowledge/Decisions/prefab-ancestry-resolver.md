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
  - docs/superpowers/specs/2026-03-25-prefab-ancestry-resolver-design.md
  - docs/superpowers/plans/2026-03-25-prefab-ancestry-resolver.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/prefab-recipe-system.md
tags: [decision, prefab-scenario-tooling]
---

# Prefab Ancestry Resolver

## Context

`game_duplicate` and `prefab_create` operated blind to what a parent prefab provides. Duplicating a base game prefab copied only the leaf file - ancestor components invisible. Creating a prefab with a `parentPrefab` used hardcoded template components instead of the actual inherited set, producing prefabs that duplicate parent components or miss required ones.

## Evidence

- Spec `2026-03-25-prefab-ancestry-resolver-design.md` contains an explicit "Decisions" table with five answered questions (see Decision below).
- The ancestry logic already existed privately in `prefab-inspect.ts` (`walkChain`, `parseComponents`); the spec's problem statement states the fix is to extract it into a shared utility.
- Implementation exists in the tree: `src/utils/prefab-ancestry.ts`, plus `prefab-inspect.ts` refactored to import from it (git history shows "refactor: prefab-inspect imports from shared prefab-ancestry utility" and "feat: extract prefab ancestry resolution into shared utility").
- Ancestry support wired into both `game_duplicate` and `prefab_create` (git history shows "feat: prefab_create resolves ancestry when parentPrefab is provided" and "feat: game_duplicate injects ancestor components and supports flatten mode").

## Inference / proposal

The Enfusion prefab model is delta-based: a child .et only stores overrides. The correct fix is to resolve the full ancestor chain and pre-populate overridable components with their original GUIDs, keeping the parent reference intact.

## Decision

- Which tools get ancestry support: both `game_duplicate` and `prefab_create`.
- How inherited components appear: keep parent reference + pre-populate overridable components with original GUIDs (Enfusion delta model).
- Auto-resolve or opt-in for `prefab_create`: opt-out - `includeAncestry` defaults to `true`, can be set `false`.
- Flatten option for `game_duplicate`: user chooses via `flatten: boolean` (default `false` = keep parent ref).
- Where shared logic lives: new `src/utils/prefab-ancestry.ts`.

## Consequences

### Benefits

- Delta-correct prefabs; no duplication of parent components; no missing required components; single shared implementation instead of tool-private copies.

### Costs and risks

- Requires reading parent files across loose files and .pak archives (readEtFile/walkChain handle this); malformed or unresolvable parent chains must be tolerated (warnings array in walkChain).

## Verification boundary

- Verified: decision table exists in spec; shared utility and both tool integrations present in tree; unit tests added for prefab-ancestry utility (git history).
- Not verified: behavior against live base-game chains re-run during vault creation; spec status is "Draft" even though implementation exists.
- Revisit trigger: any change to how GUID matching or merge precedence works across ancestor levels.

## Related notes

- [[Knowledge/Decisions/prefab-recipe-system|Prefab Recipe System]] - thin recipes depend on this resolver for inherited components.

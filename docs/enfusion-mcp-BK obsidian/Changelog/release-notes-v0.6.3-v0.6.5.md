---
type: changelog
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
addon-version: null
engine-version: null
last-reviewed: 2026-08-12
sources:
  - RELEASE_NOTES_v0.6.3.md
  - RELEASE_NOTES_v0.6.4.md
  - RELEASE_NOTES_v0.6.5.md
related:
  - docs/enfusion-mcp-BK obsidian/Changelog/index.md
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
tags: [changelog, tool-surface]
---

# Release notes v0.6.3 - v0.6.5

Summary transcribed from RELEASE_NOTES_v0.6.3.md, RELEASE_NOTES_v0.6.4.md, RELEASE_NOTES_v0.6.5.md. These are the only RELEASE_NOTES files present in the repo; later tags (v0.6.6 - v0.12.0) have no notes files.

## v0.6.3 - Upstream Merge + Internal Refactoring (2026-03-07)

- New tools from upstream: `component_search` (ScriptComponent descendants, category + event-handler filters), `wiki_read` (full wiki page content, no truncation).
- `api_search` upgrades: `format: 'tree'` ASCII inheritance visualization, inherited members from parent classes, enum-like class detection (classes with only static properties).
- Connection health tracking on all wb_* tools; handler script recovery when Workbench already running.
- Bug fixes: search ranking fix (`searchAny()` was flattening granular scores into binary), version mismatch between index.ts and package.json, inherited-array crash guard for Workbench.
- Internal: deduplicated ~300 lines into `game-paths.ts` and `dir-listing.ts`; KB index caching; replaced duplicate GUID generator.
- Version scheme note: following upstream (Articulated7/enfusion-mcp) versioning - upstream = 0.6.1, this fork = 0.6.3.

## v0.6.4 - Prefab Inspector + Component Listing Fixes (2026-03-08)

- New `prefab_inspect` tool: reads an .et prefab's full inheritance chain, merges components across ancestors (child overrides parent by component GUID), works across loose files and .pak archives.
- `wb_component list` accepts `entityIndex` for unnamed entities; empty-name guard fix; unknown component entries filtered in JS layer.
- Note: prefab_inspect output can be large for deep chains; `component_filter` parameter may come later.
- Updated handler script: `EMCP_WB_Components.c` gains entity index support and empty-name guard.

## v0.6.5 - Bug Fixes + Config Validation, Fuzzy Search, Auto-Fetch Parent Methods (2026-03-11)

- Parser/serializer: enfusion-text escape sequences (`\n`, `\t`, `\r`, `\\`, `\"`) now round-trip; `extractParamNames` no longer emits default values in modded script super() calls.
- PAK reader: bounds checks on chunk sizes and entry name lengths (malformed files error clearly instead of hanging).
- Asset search: GUID index load failures are surfaced as warnings in responses.
- Workbench tools: `scenario_create_objective` deletes already-placed entities on partial failure; `mod_create` detects pattern filename collisions before writing.
- Config semantic validation: `mod_validate` checks `.conf` class names against the API index (unknown names warned).
- Fuzzy search: all search tools fall back to Levenshtein (distance <= 1 scores 40, <= 2 scores 20) and trigram similarity (Jaccard > 0.3 scores 15) when strict matching returns fewer than 3 results.
- `script_create` auto-fetches parent overridable methods (On*, EOn*, Get*, Set*, Can*, Handle*, Do*) from the API index when no explicit methods list is given.
- Internal: new `src/utils/fuzzy.ts` (levenshtein, trigramSimilarity) with full test coverage; `generateScript` accepts `dynamicMethods`.

## Verified vs. claimed

- Verified: all three notes exist at repo root with the content summarized above.
- Not verified: whether every listed feature shipped in released artifacts (no release artifacts inspected); test counts and tool behavior were not re-run during vault creation.

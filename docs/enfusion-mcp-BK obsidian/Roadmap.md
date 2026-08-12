---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources:
  - README.md
  - RELEASE_NOTES_v0.6.3.md
  - RELEASE_NOTES_v0.6.4.md
  - RELEASE_NOTES_v0.6.5.md
  - TODO.md
  - UPGRADE_IDEAS.md
  - docs/plans/
  - docs/superpowers/
related:
  - docs/enfusion-mcp-BK obsidian/index.md
  - docs/enfusion-mcp-BK obsidian/Changelog/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
---

# Roadmap

This page is a derived view. The GitHub tracker (med-red-09812/enfusion-mcp-BK) has issues disabled, so the release lineage below is derived from git tags (2026-02 -> 2026-07) and the RELEASE_NOTES_v0.6.x.md files; open work is taken from TODO.md and UPGRADE_IDEAS.md (both marked as OPEN, not decisions).

## Current state (derived from README.md, package.json, src/server.ts)

- **Package version:** 0.10.0 (package.json; note src/index.ts still reports 0.7.1 - an observed mismatch).
- **MCP surface:** offline tools (api_search, component_search, wiki_search/read, wb_knowledge, game_browse/read, asset_search, prefab, project, mod, script_create, config_create, layout_create, server_config, animation_graph, workshop_info, building_setup, game_duplicate, scenario_create_conflict) plus live Workbench tools (wb_*) - see README.md tool tables and src/server.ts registration list.
- **Workbench plugin:** handler scripts ship in the package under mod/Scripts/WorkbenchGame/EnfusionMCP/ (20 EMCP_WB_*.c files) and are installed automatically on wb_launch.
- **Recipe system:** documented in RECIPE_SYSTEM_OVERVIEW.md / RECIPE_ARCHITECTURE.txt; 12 recipe categories + 17 variants = 29 creation paths (verified against data/recipes/*.json; the overview's own total line says "16 variants = 28 creation paths" - stale, its table lists 17); release notes track v0.6.x iterations.
- **Mod patterns:** 10 built-in templates for mod_create (README.md "Mod Patterns" section).

## Release lineage (git tags + RELEASE_NOTES files)

Source: `git tag` ordering on the clone, plus RELEASE_NOTES_v0.6.3.md, RELEASE_NOTES_v0.6.4.md, RELEASE_NOTES_v0.6.5.md.

| Version | Date | What shipped (from tag subject / release notes) |
|---------|------|-------------------------------------------------|
| v0.5.0 | 2026-02-23 | Initial tagged release |
| v0.5.1 | 2026-02-28 | entity property read/write + game dir resolution |
| v0.5.2 | 2026-02-28 | array-of-objects property editing |
| v0.6.1 | 2026-03-01 | chore(release) |
| v0.6.2 / v0.8.0 | 2026-03-05 | .aw workspace parsing, scenario slot fix, Duplicate Project guidance (same commit carries both tags) |
| v0.6.3 | 2026-03-07 | wb_knowledge tool + upstream merge: component_search, wiki_read, class hierarchy tree, connection health tracking, inherited members, enum-like detection, handler script recovery; bug fixes (search ranking, version mismatch, array crash guard); ~300 lines deduplicated into game-paths.ts / dir-listing.ts; KB index caching (KB data ships in data/kb/ - index.json + pattern .md files) |
| `0.6.4` | 2026-03-08 | prefab_inspect tool (full inheritance chain, GUID-matched merging), wb_component entityIndex support, empty-name guard fixes. Note: this tag is `0.6.4` - no `v` prefix, the only tag breaking the convention |
| v0.6.5 | 2026-03-11 | 10 bug fixes (enfusion-text escape round-trips, extractParamNames defaults, PAK bounds checks, GUID index error surfacing, scenario_create_objective cleanup, mod_create collision detection) + config semantic validation, fuzzy search (Levenshtein + trigram), script_create auto-fetch parent methods |
| v0.6.6 | 2026-03-14 | tagged; no RELEASE_NOTES file in repo |
| v0.7.0 | 2026-03-18 | tagged; no RELEASE_NOTES file in repo |
| v0.7.1 | 2026-03-19 | tagged; no RELEASE_NOTES file in repo |
| v0.9.0 | 2026-03-25 | tagged; no RELEASE_NOTES file in repo |
| v0.10.0 | 2026-03-27 | current package.json version; tagged |
| v0.11.0 | 2026-04-02 | building destruction workflow (MCP tool + plan) |
| v0.12.0 | 2026-07-13 | version bump (upstream); not in origin history |

Version scheme note (RELEASE_NOTES_v0.6.3.md): following upstream (Articulated7/enfusion-mcp) versioning - upstream = 0.6.1, this fork = 0.6.3.

## Open work (from TODO.md - OPEN, not decisions)

All TODO.md items concern `scenario_create_objective`:

- [BUG] SlotKill ends up inside Layer_AI instead of being a direct LayerTask child - reparenting via EMCP_WB_ModifyEntity may not persist correct nesting in the saved .layer file. Investigation needed on ParentEntity(false) nesting behavior.
- [BUG] `m_eActivationType` ON_TRIGGER_ACTIVATION not settable via setProperty (enum strings silently fail via SetVariableValue); must be written directly to .layer.
- [FEAT] Write the full objective hierarchy directly to the .layer file from the tool (would also solve the enum issue).
- [FEAT] Add `m_sSpawnRadius` / spawn offset support for SlotAI.
- [FEAT] Support multiple SlotAI entities under Layer_AI (`aiSpawnCount` parameter).

## Open work (from UPGRADE_IDEAS.md - OPEN, not decisions)

Ranked by impact-to-effort in the source file. Items 1-6, 8, 9, 12, 14, 15 are marked Done there; items 7, 10, 11, 13, 16-26 remain OPEN:

- 7. Pattern Composition (Mix-and-Match) - header marked "Done" but summary table not updated; treat as unconfirmed (see Discussion notes).
- 10. Dry-Run Mode for Mutation Tools.
- 11. Duplicate Code Consolidation: project_browse & game_browse (note: v0.6.3 already extracted game-paths.ts / dir-listing.ts; item may be partially done).
- 13. Method Signature Validator Tool.
- 16. Compilation Error Feedback + Log Capture.
- 17. Example Code Snippets in Patterns.
- 18. Common Pitfalls Context Injection.
- 19. Validation-Driven Fix Suggestions.
- 20. Cross-Index "Used By" Backlinks.
- 21. MODPLAN as Structured Data.
- 22. Incremental Asset Index.
- 23. Multi-Mod Workspace Support.
- 24. Diff-Based Script Patching.
- 25. Cross-Reference Validation on Write.
- 26. Component Compatibility Matrix.

See [[Knowledge/Discussions/index|Discussions]] for individual open questions.

## Design-plan documents (real sources, not decisions)

Dated plans and specs exist in docs/plans/ and docs/superpowers/ (2026-03/04). They describe implemented or planned features: scenario_create_objective (2026-03-02), vehicle animation graph KB + MCP tools (2026-03-04), wb_knowledge (2026-03-07), v0.7.0 bugfixes (2026-03-09), full codebase review (2026-03-14), Workbench API expansion (2026-03-19), animation editor improvements (2026-03-22), driver seat jiggle (2026-03-22), prefab ancestry resolver (2026-03-25), tool discoverability (2026-03-27), prefab recipe system (2026-03-31), building destruction workflow (2026-04-01). These are cited in [[Developer/index|Developer Docs]].

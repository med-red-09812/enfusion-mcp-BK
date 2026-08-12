---
type: developer-doc
audience: developer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources:
  - README.md
  - RECIPE_SYSTEM_OVERVIEW.md
  - RECIPE_ARCHITECTURE.txt
  - src/server.ts
  - src/config.ts
  - docs/superpowers/
  - docs/plans/
related:
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/index.md
  - docs/enfusion-mcp-BK obsidian/Changelog/index.md
---

# Developer Documentation

Architecture and implementation notes for the enfusion-mcp-BK server, derived from the README, src/, and the dated plan/spec documents. The GitHub tracker has issues disabled, so the planning landscape lives in committed docs (docs/plans/, docs/superpowers/) and tracked files (TODO.md, UPGRADE_IDEAS.md).

## Architecture (from README.md, src/server.ts, src/config.ts)

- **Server entry:** `src/index.ts` creates the McpServer, loads config, calls `registerTools()`, connects via StdioServerTransport. Note: the MCP server name/version reported here is "0.7.1" while package.json is 0.10.0 (observed mismatch).
- **Tool registration:** `src/server.ts` - `registerTools()` wires ~40 tool registration functions, 2 prompts (create-mod, modify-mod), and 3 resources (class, pattern, group). Tools are grouped into phases (0 = search, 1 = generation, 3 = config, 4 = Workbench live control, plus base-game access and scenario/animation/building tools).
- **Config:** `src/config.ts` - layered precedence (defaults -> package-local enfusion-mcp.config.json -> ~/.enfusion-mcp/config.json -> environment variables). Env vars: ENFUSION_PROJECT_PATH, ENFUSION_WORKBENCH_PATH, ENFUSION_GAME_PATH, ENFUSION_WORKBENCH_HOST, ENFUSION_WORKBENCH_PORT (README.md Configuration table).
- **Search engine:** `src/index/` - SearchEngine loads scraped class/wik/group data from `data/api/` (arma-classes.json, enfusion-classes.json, groups.json, hierarchy.json), builds method/enum/property/component indexes, supports fuzzy fallback (src/utils/fuzzy.ts: levenshtein + trigram) and class-tree rendering (api-search format:tree).
- **Templates & generation:** `src/templates/` - script.ts (7 script types), prefab.ts (recipe-driven, see below), layout.ts (5 layout types), config.ts, gproj.ts, scenario.ts, server-config.ts, localization.ts.
- **Recipe system:** `src/templates/recipe.ts` (schema), `src/templates/recipe-loader.ts` (loader/cache/merge), `data/recipes/*.json` (12 categories, 16 variants), consumed by `src/tools/prefab.ts`. Full design in RECIPE_SYSTEM_OVERVIEW.md and RECIPE_ARCHITECTURE.txt.
- **Workbench client:** `src/workbench/client.ts` - TCP client for the Workbench NET API; each rawCall opens a fresh connection; call() auto-launches Workbench and installs handler scripts when needed. Protocol in src/workbench/protocol.ts.
- **Workbench handler scripts (mod side):** `mod/Scripts/WorkbenchGame/EnfusionMCP/` - 20 EMCP_WB_*.c files (GetState, CreateEntity, ModifyEntity, ListEntities, ScriptEditor, Localization, Prefabs, Resources, Layers, Clipboard, etc.) that run inside Workbench and bridge to the TS client. Installed automatically on wb_launch (README.md "Workbench Plugin").
- **Base game access:** `src/pak/reader.ts` + `src/pak/vfs.ts` - transparent reading of loose files and .pak archives; used by game_browse / game_read / asset_search / prefab ancestry.
- **Animation module:** `src/animation/` - parsers/formatters/validators for AGR/AGF/AST/ASI/AW animation graph files; tools in src/tools/animation-graph.ts.
- **Scraper pipeline:** `src/scraper/` + `scripts/` - scrapes API index from Workbench docs (doxygen-parser, source-local/remote, writer); `npm run scrape` per README.md.
- **Tests:** `tests/` - vitest suites (formats, index, pak, scraper, templates, tools, utils, workbench, animation); README.md states 187 tests.

## Module map (from src/)

- src/tools/: api-search, component-search, wiki-search, wiki-read, project, mod, script-create, prefab, config-create, server-config, layout-create, game-browse, game-read, asset-search, game-duplicate, workshop-info, animation-graph, building-setup, wb-* (launch, connect, diagnose, reload, editor, execute-action, entities, components, terrain, layers, resources, prefabs, clipboard, script-editor, localization, projects, validate, state, scenario).
- src/patterns/: loader.ts - 10 mod pattern definitions (data/patterns/*.json) for mod_create.
- src/prompts/: create-mod.ts, modify-mod.ts - guided prompt workflows (/create-mod, /modify-mod).
- src/resources/: class-resource, group-resource, pattern-resource - enfusion://class, enfusion://group, enfusion://pattern MCP resources.
- src/utils/: fuzzy.ts, game-paths.ts, dir-listing.ts, prefab-ancestry.ts, safe-path.ts, logger.ts.
- src/formats/: enfusion-text.ts (Enfusion text serialization parser/serializer), guid.ts.

## Planning entry points (GitHub tracker is disabled; committed docs are authoritative)

- docs/plans/ - dated implementation plans + designs (2026-03-02 through 2026-03-19): scenario_create_objective, vehicle animation graph KB + MCP, wb_knowledge, v0.7.0 bugfixes, full codebase review, Workbench API expansion.
- docs/superpowers/ - dated plans + specs (2026-03-19 through 2026-04-01): workbench API expansion, animation editor improvements, driver seat jiggle, prefab ancestry resolver, tool discoverability, prefab recipe system, building destruction workflow.
- TODO.md - open bugs/features (scenario_create_objective layer-file write, spawn radius, multiple SlotAI).
- UPGRADE_IDEAS.md - 26 ranked upgrade ideas with done/open status; data-quality summary (85% empty class briefs, 88% empty method descriptions, 0 scraped enum values).
- RECIPE_SYSTEM_OVERVIEW.md / RECIPE_ARCHITECTURE.txt - recipe system design and architecture layers.
- RELEASE_NOTES_v0.6.x.md - version history (v0.6.3, v0.6.4, v0.6.5).

## Planned doc pages (created as work activates)

- _(none yet - add pages here as they are created; candidate topics: Workbench protocol deep-dive, recipe system authoring guide, animation graph internals, scraper pipeline details)_

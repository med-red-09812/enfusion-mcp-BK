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
  - .github/workflows/ci.yml
  - contributing.md
  - LICENSE
  - tsconfig.json
  - tsconfig.build.json
  - enfusion-mcp.config.example.json
  - data/kb/
  - data/wiki/
related:
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/index.md
  - docs/enfusion-mcp-BK obsidian/Changelog/index.md
---

# Developer Documentation

Architecture and implementation notes for the enfusion-mcp-BK server, derived from the README, src/, and the dated plan/spec documents. The GitHub tracker has issues disabled, so the planning landscape lives in committed docs (docs/plans/, docs/superpowers/) and tracked files (TODO.md, UPGRADE_IDEAS.md).

## Architecture (from README.md, src/server.ts, src/config.ts)

- **Server entry:** `src/index.ts` creates the McpServer, loads config, calls `registerTools()`, connects via StdioServerTransport. Note: the MCP server name/version reported here is "0.7.1" while package.json is 0.10.0 (observed mismatch).
- **Tool registration:** `src/server.ts` - `registerTools()` wires 50 distinct tools via 40 register* tool functions, plus 2 prompts (create-mod, modify-mod) and 3 resources (class, pattern, group). Tools are grouped into phases (0 = search, 1 = generation, 3 = config, 4 = Workbench live control, plus base-game access and scenario/animation/building tools).
- **Config:** `src/config.ts` - layered precedence (defaults -> package-local enfusion-mcp.config.json -> ~/.enfusion-mcp/config.json -> environment variables). Env vars: ENFUSION_PROJECT_PATH, ENFUSION_WORKBENCH_PATH, ENFUSION_GAME_PATH, ENFUSION_WORKBENCH_HOST, ENFUSION_WORKBENCH_PORT (README.md Configuration table).
- **Search engine:** `src/index/` - SearchEngine loads classes/groups from `data/api/` (arma-classes.json, enfusion-classes.json, groups.json) and wiki pages from `data/wiki/pages.json`, builds method/enum/property/component indexes, supports fuzzy fallback (src/utils/fuzzy.ts: levenshtein + trigram) and class-tree rendering (api-search format:tree).
- **Templates & generation:** `src/templates/` - script.ts (7 script types), prefab.ts (recipe-driven, see below), layout.ts (5 layout types), config.ts, gproj.ts, scenario.ts, server-config.ts, localization.ts.
- **Recipe system:** `src/templates/recipe.ts` (schema), `src/templates/recipe-loader.ts` (loader/cache/merge), `data/recipes/*.json` (12 categories, 16 variants), consumed by `src/tools/prefab.ts`. Full design in RECIPE_SYSTEM_OVERVIEW.md and RECIPE_ARCHITECTURE.txt.
- **Workbench client:** `src/workbench/client.ts` - TCP client for the Workbench NET API; each rawCall opens a fresh connection; call() auto-launches Workbench and installs handler scripts when needed. Protocol in src/workbench/protocol.ts. `src/workbench/status.ts` - formatConnectionStatus() footer (connected / play / edit mode) appended to all wb_* tool responses.
- **Workbench handler scripts (mod side):** `mod/Scripts/WorkbenchGame/EnfusionMCP/` - 20 EMCP_WB_*.c files (GetState, CreateEntity, ModifyEntity, ListEntities, ScriptEditor, Localization, Prefabs, Resources, Layers, Clipboard, etc.) that run inside Workbench and bridge to the TS client. Installed automatically on wb_launch (README.md "Workbench Plugin").
- **Base game access:** `src/pak/reader.ts` + `src/pak/vfs.ts` - transparent reading of loose files and .pak archives; used by game_browse / game_read / asset_search / prefab ancestry.
- **Animation module:** `src/animation/` - parsers/formatters/validators for AGR/AGF/AST/ASI/AW animation graph files; tools in src/tools/animation-graph.ts.
- **Scraper pipeline:** `src/scraper/` + `scripts/` - scrapes API index from Workbench docs (doxygen-parser, source-local/remote, writer); `npm run scrape` per README.md.
- **Tests:** `tests/` - vitest suites (formats, index, pak, scraper, templates, tools, utils, workbench, animation); README.md states 187 tests.

## Module map (from src/)

- src/tools/: api-search, component-search, wiki-search, wiki-read, project, mod, script-create, prefab, config-create, server-config, layout-create, game-browse, game-read, asset-search, game-duplicate, workshop-info, animation-graph, building-setup, scenario-create (consolidated scenario_create: base + objective merged in 748fd27), wb-* (launch, connect, diagnose, reload, editor, execute-action, entities, components, terrain, layers, resources, prefabs, clipboard, script-editor, localization, projects, validate, state, scenario, entity-duplicate, knowledge), plus kb-loader.ts - support module for wb_knowledge (loads data/kb/index.json, tokenizes queries, keyword-overlap scoring).
- src/patterns/: loader.ts - 10 mod pattern definitions (data/patterns/*.json) for mod_create.
- src/prompts/: create-mod.ts, modify-mod.ts - guided prompt workflows (/create-mod, /modify-mod).
- src/resources/: class-resource, group-resource, pattern-resource - enfusion://class, enfusion://group, enfusion://pattern MCP resources.
- src/utils/: fuzzy.ts, game-paths.ts, dir-listing.ts, prefab-ancestry.ts, safe-path.ts, logger.ts.
- src/formats/: enfusion-text.ts (Enfusion text serialization parser/serializer), guid.ts.

## Repo infrastructure

- **CI:** `.github/workflows/ci.yml` - single "CI" workflow (job `build-and-test`) triggered on push/PR to master and main; runs on ubuntu-latest with a node-version matrix of 20 and 22; steps: actions/checkout@v4, actions/setup-node@v4 with npm cache, `npm ci`, `npm run build`, `npm test`.
- **Contributing:** `contributing.md` - issue-first policy: open an issue before a PR for anything beyond small bug fixes; contributors need Node.js 20+ (Arma Reforger Tools only for live wb_* testing); PRs must be one feature/fix each, pass `npm test` (187 tests), build with no TypeScript errors (strict mode), and add tests for new functionality; contributions are MIT-licensed per the License section.
- **License:** `LICENSE` - MIT License, Copyright (c) 2025 enfusion-mcp contributors.
- **Build config:** `tsconfig.json` - target ES2022, module/moduleResolution Node16, strict, esModuleInterop, skipLibCheck, forceConsistentCasingInFileNames, resolveJsonModule, declaration + declarationMap + sourceMap, outDir dist, includes src/scripts/tests. `tsconfig.build.json` extends it with rootDir ./src, outDir ./dist, and includes only `src/**/*` (excludes tests/scripts) - the production build emits dist/ from src/ alone.
- **Config example:** `enfusion-mcp.config.example.json` - minimal surface: workbenchPath, projectPath, dataDir (default ./data), patternsDir (default ./data/patterns). Precedence (src/config.ts): defaults -> package-local enfusion-mcp.config.json -> ~/.enfusion-mcp/config.json -> environment variables.

## Data surfaces

- **data/kb/ - wb_knowledge knowledge base:** index.json (prebuilt routing map, 40 entries of path/title/description/keywords) plus 48 pattern .md files under patterns/ in 12 category dirs (Scripting_And_Core, GameModes_And_Scenarios, Modding_And_Extensions, Tools_And_Workbench, Vehicles_And_Physics, Weapons_And_Attachments, UI_And_Interface, Audio_And_Sound, AI_And_Behavior, Terrain_And_Environment, Inventory_And_Damage, Character_And_Animation - the last with a nested animation/ subfolder). Consumed by src/tools/kb-loader.ts (tokenize + keyword-overlap scoring, read-on-demand, no preload/cache) and exposed via the wb_knowledge tool (src/tools/wb-knowledge.ts).
- **data/wiki/ - wiki_search / wiki_read source:** export.xml (MediaWiki export-0.11 XML of the Bohemia Interactive Community wiki, ~5.3 MB) and pages.json (258 articles as {title, source: bistudio-wiki, content, url}).

## Planning entry points (GitHub tracker is disabled; committed docs are authoritative)

- docs/plans/ - dated implementation plans + designs (2026-03-02 through 2026-03-19): scenario_create_objective, vehicle animation graph KB + MCP, wb_knowledge, v0.7.0 bugfixes, full codebase review, Workbench API expansion.
- docs/superpowers/ - dated plans + specs (2026-03-19 through 2026-04-01): workbench API expansion, animation editor improvements, driver seat jiggle, prefab ancestry resolver, tool discoverability, prefab recipe system, building destruction workflow.
- TODO.md - open bugs/features (scenario_create_objective layer-file write, spawn radius, multiple SlotAI).
- UPGRADE_IDEAS.md - 26 ranked upgrade ideas with done/open status; data-quality summary (85% empty class briefs, 88% empty method descriptions, 0 scraped enum values).
- RECIPE_SYSTEM_OVERVIEW.md / RECIPE_ARCHITECTURE.txt - recipe system design and architecture layers.
- RELEASE_NOTES_v0.6.x.md - version history (v0.6.3, v0.6.4, v0.6.5).

## Committed designs (docs/plans/, docs/superpowers/)

One-to-two-line substance per design document that is otherwise only name-listed above (dates are file prefixes):

- **2026-03-04 vehicle animation graph KB + MCP** (docs/plans/2026-03-04-vehicle-animation-graph-kb-mcp(-design).md): restructure animation knowledge into a navigable subfolder of focused Markdown files and add three MCP tools (`animation_graph_inspect`, `animation_graph_author`, `animation_graph_setup`) that parse Enfusion text serialization directly with no Workbench connection. Landed as src/animation/ + the consolidated animation-graph tool.
- **2026-03-07 wb_knowledge** (docs/plans/2026-03-07-wb-knowledge(-design).md): bundle the KB pattern .md files into data/kb/patterns/ with a prebuilt data/kb/index.json routing map; kb-loader.ts tokenizes the query, scores entries by keyword overlap, and reads the top-N matched files on demand - no pre-loading, no caching, no SearchEngine changes.
- **2026-03-09 v0.7.0 bugfixes + features** (docs/plans/2026-03-09-v070-bugfixes-and-features(-design).md): 10 bug fixes (enfusion-text escape round-trips, socket double-processing, PAK reader bounds checks, GUID index error surfacing, scenario_create_objective cleanup, mod_create collision detection, and more) plus 3 features (config semantic validation, fuzzy search, script_create auto-fetch parent methods); 7 independent parallel work streams.
- **2026-03-14 full codebase review** (docs/plans/2026-03-14-full-codebase-review.md): 6 parallel agents reviewed all 81 source + 19 test files and found 8 critical / 12 high / 22 medium / 16 low issues; top priorities were missing edit-mode guards on destructive Workbench tools, arbitrary command execution via wb_execute_action, path traversal gaps (mod_validate, mod_build, wb_cleanup), memory-exhaustion vectors (PAK reader, TCP client), and tautological tests. Follow-ups landed as commits 166022d and 71dee64 (see below).
- **2026-03-19 Workbench API expansion** (docs/plans/2026-03-19-workbench-api-expansion-design.md + docs/superpowers/plans/2026-03-19-workbench-api-expansion.md): based on IDA Pro analysis of ArmaReforger_Workbench.exe (stable 1.4.0) - 8 items (1 bug fix, 6 new actions on existing tools, 1 new tool); each item pairs an EnforceScript handler (mod/Scripts/WorkbenchGame/EnfusionMCP/EMCP_WB_*.c) with a TypeScript registration (src/tools/wb-*.ts) over the JsonApiStruct/NetApiHandler TCP pattern; v0.8.0 target, no breaking changes.
- **2026-03-22 animation editor improvements** (docs/superpowers/plans/ + specs/2026-03-22-animation-editor-improvements-design.md): deep AGF/AGR/AST/ASI/AW parsing into a typed node tree (shared src/animation/parser.ts), V01-V13 validation checks, suggestion logic, and guide presets for character/weapon/prop in addition to vehicles; existing animation_graph_inspect/setup gain new actions and richer output - no new tools.
- **2026-03-22 driver seat jiggle** (docs/superpowers/plans/2026-03-22-driver-seat-jiggle.md + specs/2026-03-22-driver-spine-jiggle-design.md): add one AnimSrcNodeProcTrBoneItem to the existing Jiggles node's Bones block in the M151A2 Body sheet so the driver_idle seat socket bounces with suspension, driven by existing suspension_0..3 variables; one .agf file only, no AGR/structural changes.
- **2026-04-01 building destruction workflow** (docs/superpowers/plans/2026-04-01-building-destruction-workflow.md): 4 new Blender operators in bk_building_tools/operators/destruction.py (fracture, suggest removal, finalize phase, export building) plus a new building-setup MCP tool (src/tools/building-setup.ts) that reads the exported JSON manifest to auto-create structure/part prefabs with SlotBoneMappingObject / BaseSlotComponent wiring and SCR_DestructionMultiPhaseComponent.

## Substance commits (audit/review follow-ups)

- **71dee64 (2026-03-18) "22 bug fixes from full codebase audit":** search-engine fixes (getInheritedMembers dedupe across chain, fuzzy fallback added to searchEnums/searchProperties, O(n^2) -> Set in searchClasses/searchMethods fuzzy, unused hierarchy.json load removed); Workbench client resets state and calls cleanup() on TIMEOUT/PROTOCOL_ERROR; GUID regex matches lowercase hex, parsePos NaN guard, MOB starting supplies 3000 -> 500, placeholder harbor GUID replaced with real T3 GUID; wb_cleanup path safety, wb-scenario shared helpers + missing layer file-exists checks, dead parseNodeMultiIdent removed.
- **166022d (2026-03-14) "16 bug fixes from full codebase review - security, safety, correctness":** requireEditMode guards added to wb_entity_duplicate, scenario_create_objective, wb_execute_action (with blocklist), wb_script_editor mutations, wb_localization, wb_resources; MAX_STRING_LENGTH (16 MB) protocol-decoder and MAX_RESPONSE_SIZE (10 MB) TCP guards; path traversal protection in mod_validate and wb_cleanup modDir; empty-TCP-response rejection, socket.end(requestBuf) flush; wb_entity_duplicate create-then-delete reorder; animation-graph regex fix (skipped letters n/r), wheelCount even-number validation, workshop-info basePath guard.
- **ff020b2 (2026-03-12) "security hardening - path traversal fixes + remove duplicate data":** validateProjectPath() in wb_entity_duplicate destPath and animation_graph_inspect game-source branch; containment check in game-paths.ts resolveAddonDir for user-supplied modName; removed data/knowledge/ (unused duplicate of data/kb/) and test-guid.mjs (dev utility with hardcoded personal paths); version string fixes (index.ts 0.6.4 -> 0.6.5).

## Planned doc pages (created as work activates)

- _(candidate topics as pages get spun out: Workbench protocol deep-dive, recipe system authoring guide, animation graph internals, scraper pipeline details, per-design notes from the committed designs above)_

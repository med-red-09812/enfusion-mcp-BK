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
  - docs/superpowers/specs/2026-03-27-tool-discoverability-design.md
  - docs/superpowers/plans/2026-03-27-tool-discoverability.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
---

# Tool Consolidation 54 -> 47

## Context

The MCP surface had 54 tools, all deferred (Claude sees only names at session start, not descriptions). Similar names (`game_duplicate` vs `wb_entity_duplicate`, `prefab_create` vs `prefab_inspect`) created ambiguity, and the LLM had to guess which tool to call.

## Evidence

- Design `2026-03-27-tool-discoverability-design.md` states the goal: reduce tool count from 54 to 47 by merging related families, plus add a session-start routing file.
- The plan maps each merge: animation_graph (3 -> 1), prefab (2 -> 1), project (3 -> 1), mod (3 -> 1), scenario_create (partial, 3 -> 2).
- Implementation exists in the tree: `src/tools/animation-graph.ts`, `src/tools/prefab.ts`, `src/tools/project.ts`, `src/tools/mod.ts` are multi-action tools with action discriminators, and `src/tools/wb-scenario.ts` registers the consolidated `scenario_create` with `type: base|objective` (confirmed in src/server.ts registration list). `src/tools/scenario-create.ts` remains single-purpose: it registers only `scenario_create_conflict`.

## Inference / proposal

Consolidation reduces ambiguity without losing capability by giving each family a single tool name plus an `action` discriminator. Handler logic stays in dedicated files to keep them focused; a markdown routing table gives the LLM intent -> tool mapping at session start.

## Decision

- Merge animation_graph_author/inspect/setup into `animation_graph` with `action: author|inspect|setup`.
- Merge prefab_create + prefab_inspect into `prefab` with `action: create|inspect`.
- Merge project_browse/read/write into `project` with `action: browse|read|write`.
- Merge mod_build/create/validate into `mod` with `action: build|create|validate`.
- Merge scenario_create_base + scenario_create_objective into `scenario_create`; keep `scenario_create_conflict` standalone.
- Add `tools-routing.md` to the arma-knowledge folder, referenced from INDEX.md, loaded at every session start.

## Consequences

### Benefits

- 54 -> 47 tools; less ambiguity between similar names; parameters stay disjoint per action; routing file gives upfront knowledge of tool purpose.

### Costs and risks

- Existing callers using the old flat tool names must switch to the action-discriminated form (a breaking change to tool call interface); README tool tables may lag behind (README.md still lists flat names such as project_browse/mod_validate).

## Verification boundary

- Verified: multi-action tools exist in src/tools/ and are registered in src/server.ts; design spec and plan committed.
- Not verified: whether tools-routing.md was actually created in arma-knowledge (external folder, not in this repo); whether README was updated to match.
- Revisit trigger: adding a new tool family, or renaming any consolidated tool.

## Related notes

- [[Knowledge/Decisions/index|Decisions index]]

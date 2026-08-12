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
  - UPGRADE_IDEAS.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
---

# Multi-mod workspace support

## Question / scope

Should the server support a workspace model where `ENFUSION_PROJECT_PATH` points at an `addons/` directory and tools accept a `modName` parameter to select which addon to operate on? Scope: multi-addon workflows vs. the current single-path design.

## Evidence

- UPGRADE_IDEAS.md item 23 (OPEN - Tier 2): "Currently `ENFUSION_PROJECT_PATH` points to a single addon... Many modders work on multiple mods simultaneously. The current single-path design means switching mods requires restarting the MCP server or passing `projectPath` on every call."
- Notes `mod_create` already creates subdirectories under projectPath, but other tools do not navigate the workspace well.
- Proposed change location: `src/config.ts`, all tools that use `config.projectPath`.
- Listed Effort: M; Category: Modder Workflow.
- Related committed work: `ENFUSION_DEFAULT_MOD` config (git log: "feat: add ENFUSION_DEFAULT_MOD config to prevent wrong-addon selection") already exists as a partial mitigation.

## Inference / proposal

A workspace-level path with per-call modName selection would remove the restart-per-mod friction, building on the existing ENFUSION_DEFAULT_MOD mechanism.

## Alternatives and trade-offs

- Option: full workspace model (projectPath = addons dir, modName per tool).
  - Benefits: no restarts; project_browse lists all addons.
  - Costs or risks: touches every tool using config.projectPath (large refactor).
- Option: keep single path + defaultMod.
  - Benefits: current behavior, already partially solved.
  - Costs or risks: multi-mod switching still awkward.

## Open questions

- Should modName default to the existing ENFUSION_DEFAULT_MOD when omitted?
- Which tools should scope to modName vs. accept an explicit projectPath?

## Verification boundary

- Verified: item 23 open in UPGRADE_IDEAS.md; ENFUSION_DEFAULT_MOD exists (git log + src/config.ts).
- Not verified: any workspace model implementation (config still resolves a single projectPath during vault creation).
- Evidence needed next: workflow description of multi-mod modders to define the modName default rules.

## Related notes

- [[Knowledge/Discussions/index|Discussions index]]

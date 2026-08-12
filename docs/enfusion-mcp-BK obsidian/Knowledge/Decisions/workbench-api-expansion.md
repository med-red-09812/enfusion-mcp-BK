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
  - docs/plans/2026-03-19-workbench-api-expansion-design.md
  - docs/superpowers/plans/2026-03-19-workbench-api-expansion.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
---

# Workbench API Expansion Scope

## Context

IDA Pro binary analysis of `ArmaReforger_Workbench.exe` (branch `continuous_branches_stable_1.4.0`) revealed confirmed native API methods that the MCP server did not yet expose. The design spec (2026-03-19) scopes a Workbench API expansion for version target v0.8.0.

## Evidence

- Design `2026-03-19-workbench-api-expansion-design.md`: motivation states all referenced methods are confirmed present in the binary via string/RTTI analysis.
- Scope stated in the spec: 2 new files created (wb-compile.ts, EMCP_WB_Compile.c), 8 existing files changed, no breaking changes to existing tool interfaces, no new dependencies.
- Implementation plan `2026-03-19-workbench-api-expansion.md` (superpowers): 8 items = 1 bug fix (Localization getTable), 6 new actions on existing tools (Localization listLanguages, entity getWorldTransform, etc.), 1 new tool.

## Inference / proposal

Each feature has two halves: an EnforceScript handler in `mod/Scripts/WorkbenchGame/EnfusionMCP/EMCP_WB_*.c` (runs inside Workbench) and a TypeScript registration in `src/tools/wb-*.ts`, communicating over TCP with the JsonApiStruct/NetApiHandler pattern.

## Decision

- Proceed with the 8-item expansion: 1 bug fix + 6 new actions on existing tools + 1 new tool.
- Constraint: no breaking changes to existing tool interfaces.
- Constraint: no new dependencies.

## Consequences

### Benefits

- Exposes confirmed native API capabilities previously unreachable; no interface breakage for existing users.

### Costs and risks

- Two-sided maintenance: every new Workbench API needs both a .c handler and a TS registration; live Workbench tools cannot be unit tested (noted in plan).

## Verification boundary

- Verified: design and plan documents committed; scope items enumerated in both.
- Not verified: whether all 8 items shipped in a release (RELEASE_NOTES files only cover v0.6.3-v0.6.5; v0.8.0 tag exists in git but has no notes file in this tree).
- Revisit trigger: v0.8.0+ release notes, or confirmation of which items landed.

## Related notes

- [[Knowledge/Decisions/index|Decisions index]]
- [[Changelog/release-notes-v0.6.3-v0.6.5|Release notes v0.6.3 - v0.6.5]]

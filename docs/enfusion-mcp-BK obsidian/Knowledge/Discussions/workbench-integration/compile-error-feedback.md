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
tags: [discussion, workbench-integration]
---

# Compilation error feedback + log capture

## Question / scope

Should the server expose Workbench compilation errors and runtime logs to the LLM, instead of the LLM inferring failures from missing success responses? Scope: post-wb_play/wb_reload error extraction and a log-capture tool.

## Evidence

- UPGRADE_IDEAS.md item 16 (OPEN - Tier 2): "After `wb_play` or `wb_reload` fails due to compilation errors, parse the Workbench response to extract file path, line number, and error message, then automatically `project_read` the failing file and present the error in context."
- Second half: "Add a tool that reads Workbench's script compilation output and runtime `Print()` log... The Workbench NET API handler scripts (`mod/Scripts/WorkbenchGame/EnfusionMCP/`) already run inside Workbench's scripting environment and could potentially capture `Log.Info`/`Log.Error` output."
- Quotes the create-mod prompt workflow (`src/prompts/create-mod.ts:143`): "If compilation failed (errors in the Workbench console), fix with project_write." - but notes Claude has no way to see those errors. (line number as cited in the source prompt; in the current tree the compilation-failed step is at create-mod.ts:145)
- Listed Effort: M; Category: Modder Workflow. Proposed new files: `src/tools/wb-compile-errors.ts` or `src/tools/wb-log.ts`, `EMCP_WB_GetLog.c`.

## Inference / proposal

Two complementary features: parse error locations from Workbench responses (with context code), and add a log-reading tool backed by a new handler script.

## Alternatives and trade-offs

- Option: response parsing only.
  - Benefits: no new handler script.
  - Costs or risks: does not capture runtime Print() output.
- Option: full log capture with EMCP_WB_GetLog.c.
  - Benefits: complete error visibility, including runtime logs.
  - Costs or risks: new Workbench-side handler; log volume handling.

## Open questions

- Is the compilation output even available through the NET API response, or only in the Workbench console?
- How should log volume be capped for LLM context?

## Verification boundary

- Verified: item 16 open in UPGRADE_IDEAS.md; no wb-log tool or EMCP_WB_GetLog.c present in the tree during vault creation.
- Not verified: NET API capability for compile output/logs.
- Evidence needed next: a probe of Workbench's NET API responses after a failed compile.

## Related notes

- [[Knowledge/Discussions/index|Discussions index]]

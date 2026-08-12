---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources: []
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Decisions/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
  - docs/enfusion-mcp-BK obsidian/Developer/index.md
tags: [moc, workbench-integration]
---

# Workbench Integration

The live Workbench control surface (TCP client + EMCP_WB_*.c handler scripts) and the open questions around it: expansion scope, error/log visibility, verification ergonomics, multi-mod workspaces, and preview-before-write. Architecture context lives in the [[Developer/index|Developer Docs]] (Workbench client and handler script sections).

## Notes

- [[Knowledge/Decisions/workbench-api-expansion|Workbench API Expansion Scope (decision)]] - 8 items (1 bug fix, 6 new actions, 1 new tool) from IDA Pro binary analysis; no breaking changes.
- [[Knowledge/Discussions/workbench-integration/compile-error-feedback|Compilation error feedback (discussion)]] - parse compile errors from Workbench responses and capture runtime logs.
- [[Knowledge/Discussions/workbench-integration/method-signature-validator|Method signature validator (discussion)]] - cheap single-method API check, pairs with the v0.6.5 fuzzy infrastructure.
- [[Knowledge/Discussions/workbench-integration/multi-mod-workspace|Multi-mod workspace (discussion)]] - workspace model over single projectPath; ENFUSION_DEFAULT_MOD exists as partial mitigation.
- [[Knowledge/Discussions/workbench-integration/dry-run-mode|Dry-run mode (discussion)]] - preview what creation tools would write before writing, generalizing script_create's exists-check behavior.

## Areas drawn from

- [[Knowledge/Decisions/index|Decisions]]
- [[Knowledge/Discussions/index|Discussions]]
- [[Developer/index|Developer Docs]]

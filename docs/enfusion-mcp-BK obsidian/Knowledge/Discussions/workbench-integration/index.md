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
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
tags: [index, discussion, workbench-integration]
---

# Discussions: Workbench Integration

Open questions about the Workbench/API surface: error feedback, verification, workspace model, and preview-before-write. None of these are decided.

## Notes

- [[Knowledge/Discussions/workbench-integration/compile-error-feedback|Compilation error feedback + log capture]] - expose Workbench compile errors and runtime logs to the LLM instead of inferred failures.
- [[Knowledge/Discussions/workbench-integration/method-signature-validator|Method signature validator tool]] - lightweight single-method API signature check (UPGRADE_IDEAS #13).
- [[Knowledge/Discussions/workbench-integration/multi-mod-workspace|Multi-mod workspace support]] - workspace model over the single projectPath design (UPGRADE_IDEAS #23).
- [[Knowledge/Discussions/workbench-integration/dry-run-mode|Dry-run mode for mutation tools]] - preview what creation tools would write before writing (UPGRADE_IDEAS #10).

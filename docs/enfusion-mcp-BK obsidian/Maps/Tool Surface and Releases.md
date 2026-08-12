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
  - docs/enfusion-mcp-BK obsidian/Knowledge/Lessons/index.md
  - docs/enfusion-mcp-BK obsidian/Changelog/index.md
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
tags: [moc, tool-surface]
---

# Tool Surface and Releases

The MCP tool surface (consolidation, hardening lessons) and the release history that shipped it. The roadmap's release lineage is the sequence view; the changelog entry is the only note transcribed from RELEASE_NOTES files.

## Notes

- [[Knowledge/Decisions/tool-consolidation|Tool Consolidation 54 -> 47 (decision)]] - merge tool families behind action discriminators plus a session-start routing file.
- [[Knowledge/Lessons/version-mismatch|Server version must match package.json (lesson)]] - index.ts/package.json drift; observed regressed to 0.7.1 vs 0.10.0.
- [[Knowledge/Lessons/pak-bounds-checks|PAK bounds checks (lesson)]] - bound-check chunk sizes and entry name lengths against the buffer.
- [[Changelog/release-notes-v0.6.3-v0.6.5|Release notes v0.6.3 - v0.6.5 (changelog)]] - upstream merge, prefab inspector, config validation, fuzzy search.
- [[Roadmap|Roadmap]] - release lineage v0.5.0 -> v0.12.0 and open work from TODO.md / UPGRADE_IDEAS.md.

## Areas drawn from

- [[Knowledge/Decisions/index|Decisions]]
- [[Knowledge/Lessons/index|Lessons]]
- [[Changelog/index|Changelog]]
- [[Roadmap|Roadmap]]

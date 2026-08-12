---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources:
  - RELEASE_NOTES_v0.6.3.md
  - RELEASE_NOTES_v0.6.4.md
  - RELEASE_NOTES_v0.6.5.md
  - TODO.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
tags: [index, lesson]
---

# Lessons

Evidence-backed lessons transcribed from release notes (fixed bugs) and the TODO.md root-cause investigation. The GitHub tracker has issues disabled; these come from committed evidence. Status: draft.

## Entries

- [[Knowledge/Lessons/search-ranking-granularity|Preserve granular relevance scores in search ranking]] - searchAny() flattened 100/80/60 scores into binary exact-or-not, losing ranking precision (RELEASE_NOTES_v0.6.3).
- [[Knowledge/Lessons/version-mismatch|Server version must match package.json]] - index.ts and package.json version drift caused a mismatch bug, fixed in v0.6.3.
- [[Knowledge/Lessons/pak-bounds-checks|Validate PAK chunk sizes and entry name lengths]] - malformed PAK files could hang or read garbage; bounds checks added in v0.6.5.
- [[Knowledge/Lessons/scenario-slot-direct-child|Scenario Framework slots must be direct children of LayerTask]] - GetSlotTask only searches direct children; SlotKill nested inside Layer_AI fails to init (TODO.md root cause).

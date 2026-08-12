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
  - docs/enfusion-mcp-BK obsidian/Knowledge/Discussions/index.md
tags: [moc, recipe-pattern-system]
---

# Recipe / Pattern System

JSON-driven creation paths (prefab recipes, mod patterns) and the search quality that surfaces API information during authoring. The recipe decision depends on the ancestry resolver (see [[Maps/Prefab and Scenario Tooling|Prefab and Scenario Tooling]]); pattern composition is an open mod_create question.

## Notes

- [[Knowledge/Decisions/prefab-recipe-system|Prefab Recipe System (decision)]] - JSON recipes (12 categories, 17 variants) replace hardcoded PREFAB_CONFIGS; thin guidance pointing at base parent prefabs.
- [[Knowledge/Discussions/recipe-pattern-system/pattern-composition|Pattern composition (discussion)]] - whether mod_create should merge a patterns array; source status conflict (header Done vs summary table open).
- [[Knowledge/Lessons/search-ranking-granularity|Search ranking granularity (lesson)]] - searchAny() flattened granular scores to binary; keep multi-signal scores granular through aggregation.

## Areas drawn from

- [[Knowledge/Decisions/index|Decisions]]
- [[Knowledge/Lessons/index|Lessons]]
- [[Knowledge/Discussions/index|Discussions]]

---
type: developer-doc
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
last-reviewed: 2026-08-12
sources:
  - README.md
  - RECIPE_SYSTEM_OVERVIEW.md
  - docs/plans/
  - docs/superpowers/
related:
  - docs/enfusion-mcp-BK obsidian/Roadmap.md
  - docs/enfusion-mcp-BK obsidian/Developer/index.md
  - docs/enfusion-mcp-BK obsidian/Knowledge/index.md
  - docs/enfusion-mcp-BK obsidian/Changelog/index.md
---

# enfusion-mcp-BK Obsidian Vault

This folder is the curated Obsidian vault for **enfusion-mcp-BK** knowledge. The GitHub tracker (med-red-09812/enfusion-mcp-BK) has issues **disabled**, so this vault is derived from committed sources only: README.md, RECIPE_*.md, RELEASE_NOTES_v0.6.x.md, TODO.md, UPGRADE_IDEAS.md, docs/plans/, docs/superpowers/, and src/. Nothing in this vault overrides a committed design decision.

MCP server for Enfusion engine / Arma Reforger modding: API research, code generation, project scaffolding, Workbench control, and in-editor testing.

## Start here

- [[Roadmap|Roadmap]] - release lineage (v0.5.0 -> v0.12.0), current state, and open work from TODO.md / UPGRADE_IDEAS.md.
- [[Developer/index|Developer Docs]] - architecture (config, search engine, tools, Workbench client, handler scripts) and planning-doc entry points.
- [[Knowledge/index|Knowledge Library]] - decisions transcribed from design specs, lessons from release notes and investigations, and open discussions.
- [[Changelog/index|Changelog Navigation]] - v0.6.3 -> v0.6.5 release notes (only versions with RELEASE_NOTES files).

## Documentation map

- **Roadmap** - git-tag lineage with dates, current state (package.json version, tool surface, recipe system), open bugs/features (TODO.md), and open upgrade ideas (UPGRADE_IDEAS.md).
- **Developer docs** - tool registration flow (src/index.ts -> src/server.ts), configuration precedence (src/config.ts), search engine + bundled data (src/index/, data/), generation templates and recipe system (src/templates/, data/recipes/), Workbench TCP client (src/workbench/, mod/Scripts/WorkbenchGame/EnfusionMCP/), animation module, scraper pipeline, tests, and the dated plan/spec documents in docs/plans/ and docs/superpowers/.
- **Knowledge/Decisions** - five transcribed design decisions with status `review` (recipe system, ancestry resolver, tool consolidation, Workbench API expansion scope, scenario objective orchestration).
- **Knowledge/Lessons** - four evidence-backed lessons from release notes and TODO.md investigation.
- **Knowledge/Discussions** - open questions and proposals from TODO.md and UPGRADE_IDEAS.md (not decided).
- **Knowledge/Daily Notes** - no entries (no dated maintainer-log pattern exists; issues are disabled).
- **Changelog** - one entry summarizing RELEASE_NOTES_v0.6.3/0.6.4/0.6.5.md.

## Obsidian tools

- [[Templates/Decision|Decision Template]], [[Templates/Discussion|Discussion Template]], and [[Templates/Lesson|Lesson Template]].
- [[Knowledge/Daily Notes/index|Daily Notes]] - dated maintainer logs and follow-up context (currently empty).

The repository and its committed design docs, not this derived view, remain the source of truth.

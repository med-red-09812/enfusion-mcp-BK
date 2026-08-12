---
type: lesson
audience: maintainer
visibility: internal
status: draft
publish-targets: []
owner: enfusion-mcp-BK maintainers
addon-version: null
engine-version: null
last-reviewed: 2026-08-12
sources:
  - RELEASE_NOTES_v0.6.3.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Lessons/index.md
---

# Server version must match package.json

## Situation

A version mismatch existed between `index.ts` and `package.json` in the server. The MCP server reports its version to clients; drift between the two files made the reported version unreliable.

## Evidence

- RELEASE_NOTES_v0.6.3.md, Bug Fixes: "Version mismatch - `index.ts` and `package.json` now always match."
- Current tree observation (2026-08-12): `src/index.ts` still reports version "0.7.1" while `package.json` says 0.10.0 - the mismatch appears to have regressed after the v0.6.3 fix.

## Inference / proposal

Version identity should be a single source of truth (e.g. read from package.json at runtime) so the two files cannot drift again.

## Lesson

Keep the runtime-reported MCP server version and the package version in lockstep, ideally derived from one source rather than duplicated constants.

## Verification boundary

- Verified: the v0.6.3 fix is documented; current src/index.ts reads "0.7.1" vs package.json "0.10.0" (factual observation from the clone).
- Not verified: whether the mismatch is intentional or a regression.
- Regression coverage or follow-up: none documented.

## Related notes

- [[Knowledge/Lessons/index|Lessons index]]
- [[Roadmap]] (current-state note)

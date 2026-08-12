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

# Preserve granular relevance scores in search ranking

## Situation

In the v0.6.3 release, a search ranking bug was discovered in `searchAny()`: it was flattening granular scores (100/80/60) into a binary exact-or-not match, losing ranking precision.

## Evidence

- RELEASE_NOTES_v0.6.3.md, Bug Fixes: "Search ranking fix - `searchAny()` was flattening granular scores (100/80/60) into binary exact-or-not, losing ranking precision."
- The fix was shipped in v0.6.3; the search engine lives in src/index/search-engine.ts.

## Inference / proposal

Multi-signal scoring is meaningful only if the final aggregation preserves the ordering that individual scores encode. Collapsing to a boolean destroys the relative quality ordering of results and degrades the LLM's ability to pick the best match.

## Lesson

When combining multiple search signals, keep scores granular and monotonic through aggregation; never collapse to a binary match during intermediate steps.

## Verification boundary

- Verified: the bug and its fix are documented in the release notes.
- Not verified: exact before/after ranking output was not re-run during vault creation.
- Regression coverage or follow-up: none documented beyond the v0.6.3 fix.

## Related notes

- [[Knowledge/Lessons/index|Lessons index]]

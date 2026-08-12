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
  - RELEASE_NOTES_v0.6.5.md
related:
  - docs/enfusion-mcp-BK obsidian/Knowledge/Lessons/index.md
tags: [lesson, tool-surface]
---

# Validate PAK chunk sizes and entry name lengths

## Situation

The PAK reader could encounter malformed PAK files where chunk size fields exceeded the remaining file size, causing the reader to read garbage or hang.

## Evidence

- RELEASE_NOTES_v0.6.5.md, PAK Reader: "Malformed PAK files with chunk size fields exceeding the remaining file size now throw immediately with a clear error instead of reading garbage or hanging. Entry name length is also validated against the buffer size."

## Inference / proposal

Binary parsing of untrusted/arbitrary game files must validate every length field against the actual buffer bounds before use. This prevents both hangs (infinite read loops) and out-of-bounds data.

## Lesson

In binary file parsers, bound-check every size/offset field against the remaining buffer before reading; prefer immediate, clear errors over best-effort parsing.

## Verification boundary

- Verified: fix documented in v0.6.5 release notes; PAK reader lives in src/pak/reader.ts.
- Not verified: test cases for malformed PAK files were not re-run during vault creation.
- Regression coverage or follow-up: none documented beyond the v0.6.5 change.

## Related notes

- [[Knowledge/Lessons/index|Lessons index]]

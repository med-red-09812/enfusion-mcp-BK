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
---

# Pattern composition (mix-and-match)

## Question / scope

Should `mod_create` accept an array of patterns instead of a single one, merging their scripts/prefabs/configs into one scaffold? Scope: mod pattern composition; status is ambiguous in the source.

## Evidence

- UPGRADE_IDEAS.md item 7 header: "Pattern Composition (Mix-and-Match) - Done" (header struck through with a Done marker).
- BUT the same file's Summary Table row 7 is NOT struck through: "| 7 | Pattern Composition | S | Composability | L2 |" - conflicting evidence within the same document.
- Item text: "`mod_create` (`src/tools/mod-create.ts:88`) accepts one `pattern` string... The tool just needs to iterate over an array instead of a single pattern and handle name collisions in the `{PREFIX}` replacement."
- Current tree observation: `src/tools/mod.ts` (consolidated tool) still accepts a single `pattern` string - no array-of-patterns parameter found.

## Inference / proposal

The header claims done, but neither the summary table nor the current code confirms it. Treat as OPEN / unconfirmed until verified. If implemented, it should merge patterns with collision detection (related to the mod_create collision detection added in v0.6.5).

## Alternatives and trade-offs

- Option: patterns array with collision handling.
  - Benefits: "game-mode + custom-faction + hud-widget" in one scaffold.
  - Costs or risks: prefix collision semantics; larger generated scaffolds.
- Option: keep single pattern.
  - Benefits: current behavior, no change.
  - Costs or risks: composition requires multiple mod_create calls.

## Open questions

- Was item 7 ever actually implemented? (Code and summary table say no; header says yes.)
- How should `{PREFIX}` collisions between patterns be resolved?

## Verification boundary

- Verified: conflicting markers inside UPGRADE_IDEAS.md (header vs summary table); single-pattern signature in src/tools/mod.ts.
- Not verified: PR/commit evidence of implementation.
- Evidence needed next: git history search for pattern-array support (none found in the recent log during vault creation).

## Related notes

- [[Knowledge/Discussions/index|Discussions index]]

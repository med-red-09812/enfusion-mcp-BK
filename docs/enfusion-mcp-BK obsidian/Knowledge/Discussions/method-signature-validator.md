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

# Method signature validator tool

## Question / scope

Should a `script_check` tool be added that takes a class name + method signature and verifies it against the API index, returning the correct signature if there is a close match? Scope: a single-method verification tool for the LLM, separate from full api_search class dumps.

## Evidence

- UPGRADE_IDEAS.md item 13 (OPEN - Tier 2): "88% of methods lack descriptions. The LLM writes `override void OnPlayerSpawned(int playerId, IEntity entity)` but the real signature has `IEntity controlledEntity`. The prompt says 'verify every method' but there's no ergonomic single-method verification tool - `api_search` returns full class dumps, making verification expensive in tokens."
- Noted as pairing well with item 12 (fuzzy search, Done in v0.6.5) - shares the Levenshtein/fuzzy infrastructure (src/utils/fuzzy.ts).
- Listed Effort: M; Category: Hallucination Prevention.

## Inference / proposal

A lightweight signature-check tool would let the LLM verify a single method cheaply instead of re-searching the entire class, using the fuzzy matching infrastructure added in v0.6.5.

## Alternatives and trade-offs

- Option: dedicated script_check tool.
  - Benefits: cheap verification; reuses fuzzy utils.
  - Costs or risks: new tool registration and README/prompt updates.
- Option: fold into api_search as a mode.
  - Benefits: no new tool.
  - Costs or risks: api_search stays token-heavy; single-purpose ergonomics lost.

## Open questions

- Should it verify parameter types and names, or only existence + close match?
- Should it auto-suggest the corrected signature (like a "did you mean")?

## Verification boundary

- Verified: item 13 is open in UPGRADE_IDEAS.md; fuzzy utils exist in src/utils/fuzzy.ts (v0.6.5).
- Not verified: any implementation (no script-check tool in src/tools during vault creation).
- Evidence needed next: examples of hallucinated signatures from real modding sessions to define match semantics.

## Related notes

- [[Knowledge/Discussions/index|Discussions index]]

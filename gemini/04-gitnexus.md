# GitNexus — Code Intelligence

[← Back to gemini.md](./gemini.md)

Use the GitNexus MCP tools for process-aware search, 360-degree symbol context, and impact analysis. Always verify the index is fresh with `gitnexus_status` or `npx gitnexus status`.

## Always Do

- **MUST run impact analysis before editing any symbol.** Run `gitnexus_impact({target: "symbolName", direction: "upstream"})` and report the blast radius (direct callers, affected processes, risk level) to the user.
- **MUST run `gitnexus_detect_changes()` before finishing** to verify changes only affect expected symbols and execution flows.
- **MUST warn the user** if impact analysis returns HIGH or CRITICAL risk before proceeding with edits.
- When exploring unfamiliar code, use `gitnexus_query({query: "concept"})` to find execution flows instead of grepping.
- When you need full context on a specific symbol — callers, callees, which execution flows it participates in — use `gitnexus_context({name: "symbolName"})`.

## When Refactoring

- **Renaming**: MUST use `gitnexus_rename({symbol_name: "old", new_name: "new", dry_run: true})` first. Review the preview — graph edits are safe, text_search edits need manual review. Then run with `dry_run: false`.
- **Extracting/Splitting**: MUST run `gitnexus_context({name: "target"})` then `gitnexus_impact({target: "target", direction: "upstream"})` to find all external callers before moving code.
- After any refactor: run `gitnexus_detect_changes({scope: "all"})` to verify only expected files changed.

## Never Do

- NEVER edit a function, class, or method without first running `gitnexus_impact` on it.
- NEVER ignore HIGH or CRITICAL risk warnings from impact analysis.
- NEVER rename symbols with find-and-replace — use `gitnexus_rename` which understands the call graph.
- NEVER commit or finish changes without running `gitnexus_detect_changes()`.

## Tools Quick Reference

| Tool | When to use | Command |
|------|-------------|---------|
| `query`          | Find code by concept | `gitnexus_query({query: "auth validation"})` |
| `context`        | 360-degree view of one symbol | `gitnexus_context({name: "validateUser"})` |
| `impact`         | Blast radius before editing | `gitnexus_impact({target: "X", direction: "upstream"})` |
| `detect_changes` | Scope check after edits | `gitnexus_detect_changes({scope: "staged"})` |
| `rename`         | Safe multi-file rename | `gitnexus_rename({symbol_name: "old", new_name: "new", dry_run: true})` |

## Impact Risk Levels

| Depth | Meaning | Action |
|-------|---------|--------|
| d=1 | WILL BREAK — direct callers/importers | MUST update/verify these |
| d=2 | LIKELY AFFECTED — indirect deps | Should test |
| d=3 | MAY NEED TESTING — transitive | Test if critical path |

## Resources

- `gitnexus://repo/{name}/context` — Check index freshness
- `gitnexus://repo/{name}/clusters` — Functional areas
- `gitnexus://repo/{name}/processes` — Execution flows
- `gitnexus://repo/{name}/process/{name}` — Step-by-step trace

## CLI Commands

- Re-index: `npx gitnexus analyze`
- Status: `npx gitnexus status`
- Wiki: `npx gitnexus wiki`


# gitnexus

[← Back to gemini.md](./gemini.md)

Use for:
- process-aware search
- 360-degree symbol context
- blast-radius analysis before edits
- change detection after edits
- safer rename workflows
- multi-repo and grouped-repo exploration
- execution-flow tracing and repo resources

Preferred calls:
- `list_repos` first when multiple repos may be indexed.
- `query` for concept-level exploration instead of blind grep.
- `context` for a full symbol view.
- `impact` before editing any symbol.
- `detect_changes` after edits or before commit.
- `rename` with dry-run before coordinated renames.
- `group_query` / `group_status` when the task spans multiple repos.

GitNexus operating rules adapted from upstream agent guidance:
- Always run `impact` before editing an existing symbol.
- Warn clearly on high or critical blast radius.
- Use `detect_changes` to confirm the scope after refactors or larger edits.
- Use `rename` instead of raw find-and-replace for nontrivial renames.
- Use resources such as `gitnexus://repos` and `gitnexus://repo/{name}/context` when they help establish repo scope and freshness.

Rebuild/update when needed:
- `npx gitnexus analyze`
- `npx gitnexus analyze --embeddings` if semantic vectors are required and already in use
- `npx gitnexus status`
- `npx gitnexus clean --force` only if the index is clearly corrupt and a rebuild is needed

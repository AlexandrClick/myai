# Decision policy & fallback

[← Back to gemini.md](./gemini.md)

## Decision policy

- Need the smallest useful context first: use `code-review-graph`.
- Need exact symbol and dependency navigation: use `codegraph`.
- Need process-aware search, blast radius, rename safety, or multi-repo context: use `gitnexus`.
- **Совет**: Если поиск возвращает пустые результаты в новом репозитории, явно указывайте `repo_root` (для CRG) или `projectPath` (для codegraph).
- If two tools overlap, use the cheaper one first, then the more specific one:
  - review/minimal context: `code-review-graph`
  - symbol details: `codegraph`
  - flow/risk/change validation: `gitnexus`

## Fallback policy

Read files directly only when:
- the needed repo is not indexed
- the graph is stale or unhealthy
- the tool output is ambiguous
- exact implementation details are required
- the task depends on configuration or generated code not captured well by the graph

When falling back to file reads, keep the read set narrow and graph-guided.

## Pre-finish checklist

Before finalizing conclusions or code changes:
- Confirm the chosen symbol or flow with a graph tool, not only with text search.
- For edits, ensure at least one impact-oriented tool was used.
- For reviews/refactors, ensure change scope was checked.
- Mention stale or missing index explicitly if it affected confidence.

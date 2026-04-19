# Project MCP Playbook

This workspace uses three graph-backed MCP servers:
- `code-review-graph` for token-efficient review and minimal context retrieval.
- `codegraph` for symbol search, callers/callees, file structure, and fast architectural navigation.
- `gitnexus` for process-aware search, execution-flow context, impact analysis, safe rename workflows, and multi-repo exploration.

Use graph tools before broad file scans. Prefer the smallest useful context. Only read full files when graph results are insufficient, stale, or contradictory.

## Structure

- [Primary operating rules](./01-primary-rules.md)
- [code-review-graph](./02-code-review-graph.md)
- [codegraph](./03-codegraph.md)
- [gitnexus](./04-gitnexus.md)
- [Standard workflows](./05-workflows.md)
- [Decision & fallback policies](./06-policies.md)

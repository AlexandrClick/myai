# codegraph

[← Back to gemini.md](./gemini.md)

Use for:
- fast symbol lookup
- callers/callees traversal
- symbol-centric context
- file structure lookup
- impact analysis for a single symbol
- quick architecture exploration

Preferred calls:
- `codegraph_search` to find functions, classes, methods, and types.
- `codegraph_context` for quick task context.
- `codegraph_callers` and `codegraph_callees` to trace dependencies.
- `codegraph_impact` before touching a symbol with nontrivial fan-out.
- `codegraph_node` when exact source-backed symbol detail is required.
- `codegraph_status` to confirm health/index readiness.

Rebuild/update when needed:
- `codegraph init`
- `codegraph index`
- `codegraph sync`
- `codegraph status`

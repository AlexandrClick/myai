# Standard workflow by task type

[← Back to gemini.md](./gemini.md)

### 1. General codebase exploration
1. Check index readiness with `codegraph_status` and, when relevant, `list_repos`.
2. Use `codegraph_search` or GitNexus `query` to find candidate symbols or flows.
3. Use `codegraph_context` or GitNexus `context` on the best candidate.
4. Read source files only for the final verification pass.

### 2. Code review / changed branch / PR reasoning
1. Start with `get_minimal_context_tool`.
2. Run `detect_changes_tool`.
3. Use `get_review_context_tool` for focused review context.
4. Use `get_impact_radius_tool` for risky changes.
5. If flow-level understanding is needed, use GitNexus `detect_changes`, `query`, or `context`.
6. Read only the files implicated by the graph output.

### 3. Debugging
1. Start with `get_minimal_context_tool` if the bug surface is broad.
2. Search by symptom using GitNexus `query`.
3. Inspect suspect symbols with `codegraph_context` and GitNexus `context`.
4. Use `codegraph_callers` / `codegraph_callees` or `query_graph_tool` to trace data and control flow.
5. If execution-flow context matters, inspect GitNexus process resources.
6. Confirm root cause with direct source reads.

### 4. Refactoring / modification
1. Identify the target symbol with `codegraph_search` or GitNexus `query`.
2. Run GitNexus `impact` and/or `codegraph_impact` before editing.
3. Use `codegraph_callers` / `codegraph_callees` and `query_graph_tool` to map dependencies.
4. For renames, use GitNexus `rename` with dry-run first.
5. After edits, run GitNexus `detect_changes`.
6. Then read and validate the exact touched files.

### 5. Multi-repo investigation
1. Start with GitNexus `list_repos`.
2. If repos are grouped, use `group_list`, `group_status`, and `group_query`.
3. Narrow to the specific repo or flow before using repo-local tools from codegraph or code-review-graph.

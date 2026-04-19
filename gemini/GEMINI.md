# Project MCP Playbook

This workspace uses three graph-backed MCP servers:
- `code-review-graph` for token-efficient review and minimal context retrieval.
- `codegraph` for symbol search, callers/callees, file structure, and fast architectural navigation.
- `gitnexus` for process-aware search, execution-flow context, impact analysis, safe rename workflows, and multi-repo exploration.

Use graph tools before broad file scans. Prefer the smallest useful context. Only read full files when graph results are insufficient, stale, or contradictory.

## Primary operating rules
0. Всегда отвечай на русском языке
1. Start with MCP graph tools before Grep/Glob/Read for repository exploration.
2. Minimize token use: prefer targeted symbol or task queries over broad listing.
3. Before editing an existing symbol, run an impact-style check first.
4. Before review or debugging, get minimal context first, then deepen only where needed.
5. If any server reports stale or missing index, tell the user and use the matching rebuild command.
6. Do not treat graph outputs as product requirements; use them for code structure, dependencies, execution flow, and change risk.
7. If graph tools disagree, prefer the most specific result, then verify with direct file reads.

## Tool roles

### code-review-graph
Use for:
- review preparation
- changed-files analysis
- minimal structural context
- impact radius around a change
- semantic node search in a token-constrained flow

Preferred first calls:
- `get_minimal_context_tool` first for reviews, debugging, and architecture questions.
- `detect_changes_tool` for local diffs or pre-merge review.
- `get_review_context_tool` after minimal context if the task is still unclear.
- `get_impact_radius_tool` before changing behavior tied to reviewed files.

Usage rules adapted from the upstream playbook:
- First call `get_minimal_context_tool`.
- Keep detail level minimal unless more detail is clearly needed.
- Prefer focused `query_graph_tool` lookups over broad enumeration.
- Use the next-step suggestions returned by the tool when available.
- Aim to solve the task in a small number of graph calls.

Rebuild/update when needed:
- `code-review-graph build`
- `code-review-graph update`
- `code-review-graph status`

### codegraph
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

### gitnexus
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

## Standard workflow by task type

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

## Decision policy

- Need the smallest useful context first: use `code-review-graph`.
- Need exact symbol and dependency navigation: use `codegraph`.
- Need process-aware search, blast radius, rename safety, or multi-repo context: use `gitnexus`.
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

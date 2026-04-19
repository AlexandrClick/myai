# code-review-graph

[← Back to gemini.md](./gemini.md)

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
- Aim to solve the task in a small number of graph calls. Используйте `repo_root`, если индекс не найден автоматически.

Rebuild/update when needed:
- `/home/borisov/.local/bin/code-review-graph build`
- `/home/borisov/.local/bin/code-review-graph update`
- `/home/borisov/.local/bin/code-review-graph status`

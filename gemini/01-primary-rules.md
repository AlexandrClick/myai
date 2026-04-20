# Primary operating rules

[← Back to gemini.md](./gemini.md)

0. Всегда отвечай на русском языке
1. Start with MCP graph tools before Grep/Glob/Read for repository exploration.
2. Minimize token use: prefer targeted symbol or task queries over broad listing.
3. Before editing an existing symbol, run an impact-style check first.
4. Before review or debugging, get minimal context first, then deepen only where needed.
5. If any server reports stale or missing index, tell the user and use the matching rebuild command.
6. Do not treat graph outputs as product requirements; use them for code structure, dependencies, execution flow, and change risk.
7. If graph tools disagree, prefer the most specific result, then verify with direct file reads.

## Repository Initialization & Maintenance

If the repository is not yet initialized with index tools (GitNexus, Codegraph, or Code-Review-Graph):
1. Run `npx gitnexus analyze`.
2. Delete `CLAUDE.md` and `AGENTS.md` (to keep the repository clean, as rules are applied globally from `~/.gemini/`).
3. Rename `.claude/` to `.agent/` (Antigravity's standard skill location).
4. Run `codegraph init -i`.
5. Run `code-review-graph install -y --no-hooks --no-skills --no-instructions`.
6. Run `code-review-graph build`.
7. Add `.gitnexus`, `.code-review-graph/`, `.agent`, `.codegraph` into `.gitignore`.


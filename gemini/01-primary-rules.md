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

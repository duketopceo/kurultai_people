# Landscape: agent memory tools

Written 2026-10-11 from one round of web searches plus the code. The search found little on Agent Zero's plugin ecosystem or on excerpt-level citation, so this note is thin and says where it is.

## Others in the space
- **Mem0 and Mem0 MCP servers**: Mem0 is an open-source memory layer that extracts and updates memories instead of keeping raw logs; community MCP wrappers expose add/search/update/delete and a filtered search tool. [MCP-Mem0 (LangDB listing)](https://langdb.ai/app/mcp-servers/mcp-mem0-2c7b3de9-fe9e-4bfd-9e53-4b701a4a5d2d), [community server](https://github.com/walbercardoso/mcp-mem0), [overview](https://book.st-hakky.com/en/data-science/mem0-ai-agent-persistent-memory-mcp).
- **Letta, Zep**: listed alongside Mem0 in a third-party comparison that is a product pitch, so treat it as a vendor claim. [comparison listing](https://skillselion.com/mcp/tool/io.github.kira-autonoma/agent-memory-mcp).
- **Local SQLite memory MCP servers**: recall tool plus feedback tracking; self-reported results only (same listing).
- Not verified in this pass: basic-memory, and any Agent Zero memory plugins.

## What they do better
They write memory (add, update, delete). This plugin only reads from Kurultai; it never writes. Hosted options give a managed service.

## What users expect
Fast recall, source attribution, a clear connection status, no secrets in config.

## What is unique here
Read-only access to a local hybrid FTS + vector brain, results limited to excerpt-sized snippets with `source` and `source_id` so a citation can be followed back, and a people-oriented `who_knows` route.

## Findings from reading the code
- `helpers/client.py` routes queries with two regexes (`_PERSON_QUERY`, `_MEMORY_QUERY`); matching on `@`, `team`, `notes` is brittle and surprising.
- `default_config.yaml` sets a personal `test_query` default ("Luke Duke") that is hard-coded into `load_settings`.
- No tests or CI (the earlier plan #2 proposes both and is merged but not yet implemented).
- The sidebar, hero and search modal load `docs/logo.webp`, a raster logo.

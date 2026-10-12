# Roadmap

Program roadmap, 2026-10-11. Extends the merged plan in
[`docs/plans/2026-09-21-001-kurultai-people-hardening-plan.md`](docs/plans/2026-09-21-001-kurultai-people-hardening-plan.md)
(U1-U4 keep their IDs; the plan is merged but its units are not implemented). Nothing was run: `pytest` is not installed here and the plugin imports Agent Zero's `helpers.*`, which are not on this machine.

## Now

### U1 - Hermetic tests (existing plan)
Evidence: no `tests/` directory.
- **Difficulty:** medium. The plugin imports `helpers.plugins` and `helpers.secrets` from Agent Zero, so tests need those stubbed in `conftest.py`.
- **Feasibility:** none blocking; stdlib + pytest only.
- **Simpler alternative:** test only `helpers/normalize.py` and `helpers/http_util.py` (pure functions, no Agent Zero imports) first; that is about half the logic.

### U2 - CI (existing plan)
- **Difficulty:** easy. **Feasibility:** public repo, hosted Actions run; depends on U1.
- **Simpler alternative:** one job, one Python version (3.11), no matrix.

### P1 - Remove the personal test-query default
`load_settings` falls back to "Luke Duke" and `default_config.yaml` ships it.
- **Difficulty:** easy. **Feasibility:** behaviour change (a docs/config pass, not UI). **Simpler alternative:** default to an empty string and have Test connection report "no query configured".

## Next

### P2 - Replace regex routing with explicit tool choice
`is_people_query` / `is_memory_query` guess intent from words like `@`, `team`, `notes`.
- **Difficulty:** medium (behaviour change, needs tests from U1 first). **Feasibility:** prompts in `prompts/agent.system.tool.*.md` already describe when to use each tool.
- **Simpler alternative:** delete the two regexes and the `prefer_recall_for_memory` setting; let the agent pick `kurultai_recall` vs `kurultai_search`.

### U3 - Docs (existing plan): configuration and troubleshooting
- **Difficulty:** easy. **Feasibility:** none. **Simpler alternative:** fold into README sections.

### U4 - Submit to the a0-plugins index (existing plan, closes #1)
- **Difficulty:** easy. **Feasibility:** depends on the index accepting submissions; `plugin.yaml` fields must match its schema.
- **Simpler alternative:** none.

## Later

### P3 - Write path
Today the plugin is read-only (search, recall, cite). A "remember" tool would need a Kurultai write API and an approval model.
- **Difficulty:** hard. **Feasibility:** depends on the daemon exposing writes and on trust-lane rules in the parent repo.
- **Simpler alternative:** keep it read-only; leave writing to Kurultai's own ingest.

### P4 - Replace `docs/logo.webp` everywhere with the SVG mark
Started in this pass (`ui:` commits); remove the `.webp` once the owner signs off the mark. **Difficulty:** easy.

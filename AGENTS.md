# AGENTS.md

Kurultai Memory: an [Agent Zero](https://github.com/agent0ai/agent-zero) plugin
that searches, recalls and cites your Kurultai brain with excerpt-sized results.

## Layout

| Path | What it is |
|---|---|
| `plugin.yaml` | The Agent Zero manifest. `name` is the plugin id. |
| `api/` | Agent Zero API handlers (WebUI endpoints). |
| `tools/` | The tools the agent calls. |
| `helpers/` | `client.py` (HTTP to Kurultai), `errors.py`, `normalize.py`. |
| `prompts/`, `webui/`, `docs/`, `default_config.yaml` | Agent Zero surfaces and docs. |

## Traps

- **The plugin imports itself by absolute path.** Every module does
  `from usr.plugins.kurultai_people.helpers.…`. That path only exists once the
  plugin is installed under Agent Zero's plugin root (`/a0/usr/plugins/`). You
  cannot run or import this from a plain clone, and you must not rewrite the
  imports as relative ones to "fix" that — the absolute path is the runtime
  contract.
- **`api/test_connection.py` is not a test.** It is an Agent Zero
  `ApiHandler` subclass named `TestConnection`, which is what the WebUI calls to
  check reachability. **pytest will try to collect that class** because of the
  `Test` prefix and fail on it. That is expected, not a bug to fix. Do not
  rename the class to silence pytest — the name is part of the handler contract.
  This repo has no test suite.
- **Adding a module means no mirroring.** Unlike its sibling
  `openrouter_usage`, this plugin has no `tests/_site/` tree, so there is nothing
  to keep in sync when you add a helper.

## Configuration

- `KURULTAI_API_KEY` is read from **Agent Zero Secrets**, not the environment.
- `KURULTAI_PROJECT` is read from plugin config, falling back to the environment.
- `per_project_config` and `per_agent_config` are both `false` in
  `plugin.yaml`, so settings are global. Do not add per-project settings without
  a reason to change that.

## Conventions

- Bump `version` in `plugin.yaml` when you change behaviour. Agent Zero keys the
  installed copy off `name` and `version`.
- All Kurultai HTTP goes through `helpers/client.py`. Do not open a second
  client or inline `urllib` calls elsewhere.
- User-facing errors go through `helpers/errors.py:friendly_error`, which is what
  turns a rejected key into an actionable message.

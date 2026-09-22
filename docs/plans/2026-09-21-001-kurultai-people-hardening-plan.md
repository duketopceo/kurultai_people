# Plan: kurultai_people → 10/10

**Date:** 2026-09-21 · **Status:** proposed · **Depth:** lightweight
**Origin:** repo scorecard pass — engineering rigor 7, docs 5, OSS citizenship 6.

## Problem frame

Kurultai Memory is a real, working Agent Zero plugin (search / recall / cite against the
Kurultai daemon) with a good README, but it has **zero tests, zero CI, and docs that stop
at install**. Any refactor risks silent breakage, and contributors (or future-you) have no
safety net. Open issue #1 — *submit to the a0-plugins index* — is the distribution unlock.

## Scope

**In:** hermetic test suite, GitHub Actions CI, docs gaps (configuration reference,
troubleshooting), a0-plugins index submission (closes #1).
**Out:** new agent tools, changes to the Kurultai daemon/server itself, non-MIT licensing.

## Implementation units

### U1 — Hermetic test suite
**Files:** `tests/test_tools.py`, `tests/test_config.py`, `tests/conftest.py` (new)
- Tool arg-shaping for `kurultai_search` / `kurultai_recall` / `kurultai_cite` with the
  daemon HTTP layer stubbed — no live daemon required.
- `default_config.yaml` loading: defaults apply, env vars / user config override.
- Secret hygiene: assert no test fixture or committed file contains a real API key.
**Test scenarios:** search with empty results returns a graceful message (not a traceback);
recall prefers project scope when enabled; cite with unknown `source_id` errors clearly;
malformed `default_config.yaml` fails fast with the file path in the message.

### U2 — CI workflow
**Files:** `.github/workflows/ci.yml` (new)
- Run `pytest` on Python 3.10/3.11/3.12 on push + PR. Fail the workflow on test failure.
**Test scenarios:** n/a (workflow config) — verify by opening a trivial PR and watching it run.

### U3 — Docs gaps
**Files:** `docs/configuration.md`, `docs/troubleshooting.md` (new)
- Configuration: every `default_config.yaml` key documented with type, default, and env override.
- Troubleshooting: daemon unreachable, empty search results, auth failures — symptom → check → fix.
**Test scenarios:** n/a — review criterion: a new user can configure without reading source.

### U4 — Submit to a0-plugins index (closes #1)
**Files:** `plugin.yaml` (verify/extend)
- Validate `plugin.yaml` metadata against the a0-plugins index schema (name, version,
  description, tools list) and submit per that repo's contributor guide.
**Test scenarios:** schema validation passes locally before submission.

## Key decisions

- **pytest**, stdlib + pytest only — matches the plugin's zero-dependency posture.
- Tests stay **hermetic**: stub HTTP at the boundary; the suite must pass with no daemon running.
- No behavior changes in this plan — scaffolding and docs only.

## Assumptions / open questions

- Assumes the a0-plugins index accepts external submissions (see that repo's guide).
- `api/test_connection.py` is a manual script today; U1 may subsume it — decide during implementation.

---
id: HN-003
title: Guardrails
status: todo
wireframe: none
branch:
pr:
commits: []
---

## Goal
Move enforceable rules into tools, then trim prose rules that tools now enforce.

## Details
- `pre-commit` as a Poetry dev dep (ask before adding; also `import-linter`). `.pre-commit-config.yaml` hooks:
  - isort, black, flake8, mypy
  - import-linter contract: `health_nudge.data` and `health_nudge.metrics` must not import `streamlit`
  - gitleaks
  - check-added-large-files
  - block `*.parquet` and root `data/` anywhere except `tests/fixtures/` (note: `src/health_nudge/data/` is code and must stay allowed)
  - commit-msg hook requiring `^HN-\d{3}: ` (allow `Merge ...` commits)
- `.claude/settings.json`:
  - deny Read of `.streamlit/secrets.toml` and `.env`
  - deny `git push --force` / `-f`, `git push` to main, `gh pr merge`
  - PostToolUse hook (Edit/Write on `*.py`): `mamba run -n health-nudge isort <file>` + `black <file>`
- pytest test for MIN_GROUP_SIZE suppression, marked `xfail` until suppression is implemented.
- Then TRIM CLAUDE.md and `.claude/rules/`: replace rules now enforced by a tool with one line naming the tool.
- Document hook install (`pre-commit install --hook-type pre-commit --hook-type commit-msg`) in README.

## Acceptance criteria
- [ ] `mamba run -n health-nudge pre-commit run --all-files` passes.
- [ ] A commit with a bad message (e.g. "fix stuff") is rejected; output shown in PR.
- [ ] Adding a dummy `.parquet` outside `tests/fixtures/` is rejected (then removed).
- [ ] import-linter fails when `import streamlit` is added to a data/ module (demonstrated, then reverted).
- [ ] `.claude/settings.json` denies the listed reads/commands; the format hook runs on edit.
- [ ] Suppression test exists and is `xfail`.
- [ ] CLAUDE.md and rules trimmed; each removed rule replaced by one line naming the enforcing tool.
- [ ] pytest, isort --check, black --check, flake8 pass.

## Notes/decisions

---
id: HN-002
title: Project skeleton
status: todo
wireframe: none
branch:
pr:
commits: []
---

## Goal
Folder layout, config and tool configuration so feature work can start and all checks pass.

## Details
- Layout: `app.py`, `pages/`, `src/health_nudge/{__init__,config}.py`, `src/health_nudge/data/`, `src/health_nudge/metrics/`, `tests/`, `tests/fixtures/`.
- `config.py`: `MIN_GROUP_SIZE = 10` (the only definition); parquet directory read from config (e.g. `st.secrets`/env var with a documented key), never hardcoded in code. Config module must not import streamlit if data/metrics import it.
- `.streamlit/secrets.toml.example` (committed) showing the expected keys with placeholder values.
- Minimal `app.py`: title + short description.
- pyproject.toml: `[tool.black]` line-length 88, target-version py311; `[tool.isort]` profile = "black".
- `.flake8`: max-line-length = 88, extend-ignore = E203, E701, E704; flake8-annotations checks (ANN) enforced for `src/` only (e.g. `per-file-ignores` for tests/, pages/, app.py).
- One passing test (e.g. config loads, `MIN_GROUP_SIZE == 10`).

## Acceptance criteria
- [ ] Layout exists as described; CLAUDE.md layout section still accurate.
- [ ] `streamlit run app.py` starts without errors (verified headless, e.g. start + curl health endpoint, then stop).
- [ ] pytest passes (≥1 test).
- [ ] isort --check, black --check, flake8 all pass.
- [ ] No hardcoded parquet path anywhere in code.

## Notes/decisions

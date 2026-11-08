---
id: HN-001
title: Python environment
status: done
wireframe: none
branch: HN-001-python-environment
pr: https://github.com/jaihind213/health-nudge/pull/1
commits: [9549ffb, e141451, d3b19ca]
---

## Goal
A reproducible mamba + Poetry environment that every later task runs in.

## Details
- `environment.yml`: name `health-nudge`; channel conda-forge; deps `python=3.11`, `poetry` only.
- `pyproject.toml` via Poetry: package from `src/health_nudge` (`packages = [{include = "health_nudge", from = "src"}]`), python `^3.11`.
- `poetry.toml`: `[virtualenvs] create = false` so Poetry installs into the mamba env.
- Deps: streamlit, duckdb, pandas, pyarrow, plotly.
- Dev deps: pytest, black, isort, flake8, flake8-annotations, mypy.
- Create a minimal `src/health_nudge/__init__.py` so `poetry install` can install the package.
- Add fresh-machine setup steps to README.md.

## Acceptance criteria
- [x] `mamba env create -f environment.yml` creates `health-nudge`.
- [x] `mamba run -n health-nudge poetry install` succeeds and creates no separate virtualenv (`poetry env info` shows the mamba env's Python).
- [x] `mamba run -n health-nudge python -c "..."` prints Python 3.11.x and imports streamlit, duckdb, pandas, pyarrow, plotly, health_nudge; output pasted in the PR.
- [x] Dev tools run: `pytest --version`, `black --version`, `isort --version`, `flake8 --version`, `mypy --version`.
- [x] `poetry.lock` committed.
- [x] README.md has fresh-machine setup steps (install mamba, create env, poetry install, run app).

## Notes/decisions
- Poetry 2.5 writes a PEP 621 `[project]` table, where `^3.11` is invalid;
  used the equivalent `requires-python = ">=3.11,<4.0"`. Packages config stays
  under `[tool.poetry]`; dev deps live in `[dependency-groups] dev`.
- Latest versions at creation: streamlit 1.67.0, duckdb 2.0.0, pandas 3.1.1,
  pyarrow 26.0.1, plotly 7.1.0; pytest 9.2.0, black 26.10.1, isort 9.0.2,
  flake8 7.5.0, flake8-annotations 3.3.1, mypy 2.4.1.
- pytest exits 5 ("no tests ran") in this task; the first test is added in HN-002.
- `gh` CLI was not installed, so PR #1 was opened manually in the browser.
  Later tasks need `gh` (`brew install gh && gh auth login`).

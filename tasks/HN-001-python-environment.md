---
id: HN-001
title: Python environment
status: todo
wireframe: none
branch:
pr:
commits: []
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
- [ ] `mamba env create -f environment.yml` creates `health-nudge`.
- [ ] `mamba run -n health-nudge poetry install` succeeds and creates no separate virtualenv (`poetry env info` shows the mamba env's Python).
- [ ] `mamba run -n health-nudge python -c "..."` prints Python 3.11.x and imports streamlit, duckdb, pandas, pyarrow, plotly, health_nudge; output pasted in the PR.
- [ ] Dev tools run: `pytest --version`, `black --version`, `isort --version`, `flake8 --version`, `mypy --version`.
- [ ] `poetry.lock` committed.
- [ ] README.md has fresh-machine setup steps (install mamba, create env, poetry install, run app).

## Notes/decisions

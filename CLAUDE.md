# Health Nudge Analytics Dashboard

Participants receive daily nudges in a mobile app and open, like or dislike them.
They also send daily steps, sleep and other health metrics. This Streamlit
dashboard analyses nudge engagement and health metrics, **aggregates only**.

Stack: Python 3.11 · mamba (env) + Poetry (deps) · Streamlit · DuckDB · pandas ·
Plotly. Dev: pytest, black, isort, flake8, mypy.
Data: some tables in Postgres, some as parquet files on local disk.
Pages are built from wireframe images in `wireframes/`.

## Environment
- Mamba env `health-nudge`, defined in `environment.yml` (python=3.11 + poetry, conda-forge).
- Poetry manages ALL Python packages and installs INTO the mamba env
  (`poetry.toml`: `virtualenvs.create = false`). Never create a separate virtualenv.
- Never `pip install` / `mamba install` Python packages. Use `poetry add`
  (`--group dev` for dev tools). **Ask the user before adding any dependency.**
  Commit `poetry.lock`.
- The shell does not keep an activated env: ALWAYS run `mamba run -n health-nudge <cmd>`.

## Commands (available after HN-001/HN-002)
```bash
mamba env create -f environment.yml                       # create env
mamba run -n health-nudge poetry install                  # install deps
mamba run -n health-nudge streamlit run app.py            # run app
mamba run -n health-nudge pytest                          # test
mamba run -n health-nudge isort . && mamba run -n health-nudge black .   # format
mamba run -n health-nudge flake8                          # lint
```
Before saying a task is done, run pytest, isort --check, black --check and
flake8, and report the results.

## Layout
- `app.py` — Streamlit entry point.
- `pages/` — Streamlit pages; UI only.
- `src/health_nudge/data/` — all SQL and data loading (DuckDB); returns DataFrames.
- `src/health_nudge/metrics/` — metric calculations, plain functions.
- `src/health_nudge/config.py` — settings (parquet path, `MIN_GROUP_SIZE`).
- `tests/` — pytest; `tests/fixtures/` holds small synthetic data.
- `docs/` — project docs; `docs/metrics.md` is the metric glossary.
- `tasks/` — one file per task + `INDEX.md`; `_TEMPLATE.md` for new ones.
- `wireframes/` — page wireframe images, named after their task.
- `.claude/rules/` — scoped rules; `.claude/skills/` — task workflow skills.

## Architecture
- DuckDB is the single query engine: reads parquet directly and ATTACHes
  Postgres **READ-ONLY** via the postgres extension, so queries can join both.
- All SQL and data loading lives in `src/health_nudge/data/`; functions return
  pandas DataFrames; this layer never imports streamlit.
- Metrics live in `src/health_nudge/metrics/` (plain functions, no streamlit),
  defined in `docs/metrics.md`. Every page uses these shared functions —
  never redefine a metric inline.
- `pages/` only build UI: call data/metrics functions, render charts.
- Credentials in `.streamlit/secrets.toml` (gitignored). Parquet location
  comes from config, never hardcoded.
- Tests use small synthetic fixtures; never connect to real Postgres in tests.

## Privacy (health data — non-negotiable)
- Never read, print or paste real participant rows into the conversation.
  Inspect schemas, row counts and aggregates only.
- Dashboard shows aggregates only, never individual participants.
- Suppress any aggregate group with < 10 participants
  (`MIN_GROUP_SIZE = 10`, defined once in config).
- Never commit data files, secrets or `.env`. Never write to Postgres.

## Task workflow (details: `.claude/rules/git-workflow.md`)
- One task = `tasks/HN-XXX-slug.md` + branch `HN-XXX-short-slug` from `main`.
- Commits start with `HN-XXX: <what>`.
- Done = checks pass → push → `gh pr create` titled `[HN-XXX] <title>` →
  record branch/PR/commits in task file, status `in-review`, update `tasks/INDEX.md`.
- The user merges PRs. Never push to main, merge PRs or force-push.
- If a task settles something future tasks need, document it (docs/, rules) in that task.
- Skills: `new-task` (define), `next-task` (implement), `wireframe-to-page` (UI from image).

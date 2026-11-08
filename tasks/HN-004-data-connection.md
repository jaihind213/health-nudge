---
id: HN-004
title: Data connection
status: todo
wireframe: none
branch:
pr:
commits: []
---

## Goal
One DuckDB connection that reads local parquet and the Postgres tables (read-only), plus a "Data sources" check.

## Details
- `src/health_nudge/data/connection.py`: factory returning a DuckDB connection; installs/loads the postgres extension and `ATTACH`es Postgres with `READ_ONLY` using credentials passed in (from `.streamlit/secrets.toml` via the app). Postgres optional: connection works with parquet only when no credentials are given.
- Cached in the app with `st.cache_resource` (caching lives in the UI layer; data/ stays streamlit-free).
- Parquet reader helper: reads files from the configured parquet directory with parameterised paths.
- Synthetic fixtures (small parquet / in-memory tables) for nudges and health metrics.
- "Data sources" check (function + simple page or app section): lists available tables (parquet + Postgres), their column schemas and row counts. No rows displayed.
- Document discovered table names/schemas (names and types only) in `docs/data-sources.md`.

## Acceptance criteria
- [ ] Postgres ATTACH always uses READ_ONLY (asserted in a test on the generated statement; no real Postgres).
- [ ] All SQL parameterised; no hardcoded paths or credentials.
- [ ] Data sources check returns a DataFrame of table, source, columns, row_count — never rows.
- [ ] Tests run without real Postgres and cover connection, parquet helper and data sources check.
- [ ] AppTest smoke test for the data sources page/section.
- [ ] pytest, isort --check, black --check, flake8 pass.

## Notes/decisions

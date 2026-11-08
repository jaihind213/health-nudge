---
paths:
  - "src/health_nudge/data/**"
---
# Data layer rules

- DuckDB is the only query engine. Read parquet with `read_parquet(?)`;
  reach Postgres via `ATTACH ... (TYPE postgres, READ_ONLY)` so queries can join both.
- One cached connection, created by a single factory function in this package;
  never open ad-hoc connections elsewhere.
- Postgres credentials come from `.streamlit/secrets.toml` (passed in by the
  caller); parquet paths come from config. Never hardcode either.
- Parameterised SQL only: `con.execute(sql, [params])`. Never f-string or
  `.format()` user/filter input into SQL. Identifiers (table names) only from
  a fixed allow-list in code.
- Every public function returns a pandas DataFrame (`.df()`), with documented columns.
- Prefer aggregating in SQL; never return participant-level rows to pages.
- This package never imports streamlit (keep it testable without the UI).
- Never write to Postgres (no INSERT/UPDATE/DELETE/DDL).

---
paths:
  - "tests/**"
---
# Testing rules

- pytest only. Run with `mamba run -n health-nudge pytest`.
- Synthetic fixtures only (small, generated, under `tests/fixtures/` or built in
  `conftest.py`). Never copy real data into tests.
- Never connect to real Postgres. Use parquet fixtures or an in-memory DuckDB
  with tables created from synthetic DataFrames.
- Test the data and metrics layers directly (call the functions, assert on DataFrames).
- Cover `MIN_GROUP_SIZE` suppression for every aggregate that groups participants
  (a group of 9 is suppressed, 10 is shown).
- One `streamlit.testing.v1.AppTest` smoke test per page: it runs without
  exceptions with fixture data.

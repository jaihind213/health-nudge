# Privacy rules (health data — non-negotiable)

- Never read, print or paste real participant rows into the conversation.
  This includes `head()`, `SELECT *`, `LIMIT 5` samples and `print(df)` on
  real data. To understand data, inspect schemas, row counts and aggregates only.
- The dashboard shows aggregates only — never individual participants, never
  participant IDs, never per-participant tables or drill-downs.
- Suppress any aggregate group with fewer than `MIN_GROUP_SIZE` participants
  (10, defined once in `src/health_nudge/config.py`; never hardcode 10 elsewhere).
  Count distinct participants, not rows.
- Never commit data files (parquet, csv, dumps), secrets, `.streamlit/secrets.toml`
  or `.env`. Synthetic fixtures under `tests/fixtures/` are the only exception.
- Never write to Postgres: no INSERT/UPDATE/DELETE/DDL; attach it READ-ONLY.
- If a request would break one of these rules, stop and ask the user.

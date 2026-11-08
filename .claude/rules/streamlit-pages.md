---
paths:
  - "app.py"
  - "pages/**"
---
# Streamlit page rules

- Pages are thin: call `health_nudge.data` / `health_nudge.metrics` functions,
  then render. No SQL and no metric formulas in pages.
- `st.cache_resource` for the DuckDB connection; `st.cache_data` for query results
  (wrap data-layer calls; set a `ttl` where data refreshes).
- `st.session_state` keys are module-level constants (e.g. `KEY_DATE_RANGE = "date_range"`),
  never bare string literals scattered through code.
- Filters go in `st.sidebar`.
- Charts with Plotly (`st.plotly_chart`).
- Show a friendly `st.warning` when a result is empty or suppressed
  (group below `MIN_GROUP_SIZE`) instead of an empty chart or an error.
- Show aggregates only; never render participant-level tables.

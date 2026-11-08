---
name: wireframe-to-page
description: Build a Streamlit page from a task's wireframe image - inventory every element, map each to a data/metrics function and Streamlit component, confirm the mapping and ambiguities with the user before coding, then build data -> metrics -> page with tests.
---

# wireframe-to-page

1. Read the task file and open its `wireframe` image.
2. List every element: sidebar filters, KPI cards, charts (type, axes, grouping),
   tables, text, layout (columns/tabs).
3. For each element, map to:
   - a data function in `src/health_nudge/data/` (existing or new; SQL/tables used),
   - a metric function in `src/health_nudge/metrics/` (existing or new; must match
     `docs/metrics.md`),
   - a Streamlit/Plotly component (`st.metric`, `st.plotly_chart`, `st.dataframe` of
     aggregates, `st.sidebar.*`).
4. Flag ambiguities: undefined metrics, unclear grouping, anything that would show
   individual participants, groups likely below MIN_GROUP_SIZE.
5. Show the mapping table and questions to the user. WAIT for confirmation before coding.
6. Build bottom-up: data functions + tests (synthetic fixtures) → metric functions +
   tests (incl. MIN_GROUP_SIZE suppression) → page in `pages/` (thin, cached,
   sidebar filters, `st.warning` on empty/suppressed) → AppTest smoke test.
7. Add any new metric definitions to `docs/metrics.md`.
8. Compare the running page against the wireframe and note deviations in the task's
   Notes/decisions.

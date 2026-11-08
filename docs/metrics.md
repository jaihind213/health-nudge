# Metric glossary

Single source of truth for metric definitions. Each metric is implemented once
in `src/health_nudge/metrics/` and every page uses that function.
Any aggregate over participants is suppressed when the group has fewer than
`MIN_GROUP_SIZE` distinct participants.

| Metric | Definition | Formula | Source tables | Notes |
|--------|------------|---------|---------------|-------|
| Open rate | Share of sent nudges that were opened | opened nudges / sent nudges | TO CONFIRM | TO CONFIRM: count per nudge or per participant-day? multiple opens? |
| Like rate | Share of opened nudges that were liked | liked nudges / opened nudges | TO CONFIRM | TO CONFIRM: denominator opened or sent? |
| Dislike rate | Share of opened nudges that were disliked | disliked nudges / opened nudges | TO CONFIRM | TO CONFIRM: denominator opened or sent? can a nudge be liked and disliked? |

<!-- Add rows as metrics are confirmed. Remove "TO CONFIRM" only once the user agrees. -->

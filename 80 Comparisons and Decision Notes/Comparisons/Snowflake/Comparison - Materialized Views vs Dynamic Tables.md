---
tags:
  - note-comparison
---

# Comparison - Materialized Views vs Dynamic Tables

> Materialized views speed up repeated query patterns; dynamic tables maintain transformation outputs.

## Short Answer

Use a **materialized view** when a repeated expensive query pattern on one base table should be accelerated with a maintained precomputed result. Use a **dynamic table** when the client needs a maintained derived table or pipeline with freshness targets, dependencies, and more flexible transformation logic.

## Comparison Table

| Dimension | Materialized view | Dynamic table |
|---|---|---|
| Primary purpose | Query acceleration | Managed transformation pipeline |
| Mental model | Maintained cache/precomputed result | Declarative table refresh pipeline |
| Query shape | One base table, restricted SQL | More flexible transformations and dependencies |
| Joins in definition | Not supported | Common pattern when supported by refresh mode |
| Freshness | Always returns current results; maintenance can be behind internally | Target lag defines desired staleness |
| Refresh compute | Snowflake background maintenance credits | Assigned warehouse runs refreshes |
| Best fit | Repeated expensive filters/aggregations on one large table | Multi-step derived data for BI/ELT |
| Main risk | Maintenance/storage cost outweighs read savings | Target lag, refresh failures, full refreshes, and reinitialization cost |

## Decision Rules

- If the client says "this same one-table aggregation is expensive every time," evaluate a materialized view.
- If the client says "we need Snowflake to maintain this transformed dataset/pipeline," evaluate dynamic tables.
- If joins are required inside the maintained object, a materialized view is not the right object.
- If exact freshness language matters, be careful: dynamic table target lag is a staleness goal, not a hard refresh interval.
- Use Query Profile and Query History first; do not create either object without proving repeated workload value.
- Monitor maintenance/refresh cost after rollout, not just query runtime improvement.

## Related Learning Topics

- [[01 Snowflake/02 Performance and Optimization/07 Materialized Views]]
- [[01 Snowflake/02 Performance and Optimization/08 Dynamic Tables]]
- [[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/02 Performance and Optimization/11 Result Caching]]
- [[01 Snowflake/04 Data Engineering/20 Streams and Tasks]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Dashboards Are Slow During Business Hours]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Query Scans Too Much Data]]

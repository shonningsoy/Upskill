---
tags:
  - note-comparison
---

# Comparison - Bigger Warehouse vs Clustering

> Bigger warehouses add compute; clustering improves how much data Snowflake can skip.

## Short Answer

Use a **bigger warehouse** when the workload needs more compute for the data it must process. Use **clustering** when large tables scan too much data because pruning is weak against common filters.

## Comparison Table

| Dimension | Bigger warehouse | Clustering |
|---|---|---|
| Primary problem | Not enough compute for a query/job | Too many micro-partitions scanned |
| Main effect | More processing power | Better data organization for pruning |
| Best fit | Heavy transformations, complex joins, backfills | Large tables with stable date/region/customer filters |
| Cost risk | Higher compute credits while running | Ongoing clustering maintenance credits |
| Diagnostic signal | Query is compute-heavy after reasonable pruning | Query Profile shows poor pruning/large scans |

## Decision Rules

- Start with Query Profile before recommending either lever.
- If query logic scans most of a large table, a bigger warehouse may only make an inefficient scan faster.
- If filters are selective but pruning is weak, clustering may reduce scanned data.
- Do not cluster small or low-value tables just because a query is slow.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Query Scans Too Much Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Dashboards Are Slow During Business Hours]]

---
tags:
  - note-scenario
---

# Scenario - Query Scans Too Much Data

> Client says: "This dashboard filter should be selective, but Snowflake still scans a lot."

## Likely Reasoning Path

1. Open Query Profile and inspect bytes scanned, pruning behavior, joins, and filters.
2. Check whether predicates align with micro-partition metadata such as date, region, or customer keys.
3. If the query is a highly selective lookup on IDs, UUIDs, emails, serial numbers, or semi-structured paths, consider Search Optimization Service.
4. If the table is large and filters are stable range/slice patterns, consider clustering.
5. If the scan is already well-pruned but still large (big table, selective filters, scan-bound), consider Query Acceleration Service to parallelize the remaining scan work with serverless burst compute.
6. If the table is small or predicates are not selective, clustering and search optimization are unlikely to be the right first move.
7. Avoid masking the issue by only increasing warehouse size.

## Consultant Recommendation Shape

First prove that table access is the problem. Use Search Optimization Service for repeated high-cardinality point lookups; use clustering when better table organization improves pruning for stable filters. Both add maintenance cost, so validate with Query Profile and cost monitoring.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/02 Performance and Optimization/09 Search Optimization Service]]
- [[01 Snowflake/02 Performance and Optimization/10 Query Acceleration Service]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Bigger Warehouse vs Clustering]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Search Optimization Service vs Clustering]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - QAS vs Warehouse Upsizing]]

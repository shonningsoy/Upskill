# Scenario - Query Scans Too Much Data

> Client says: "This dashboard filter should be selective, but Snowflake still scans a lot."

## Likely Reasoning Path

1. Open Query Profile and inspect bytes scanned, pruning behavior, joins, and filters.
2. Check whether predicates align with micro-partition metadata such as date, region, or customer keys.
3. If the table is large and filters are stable, consider clustering.
4. If the table is small or predicates are not selective, clustering is unlikely to be the right first move.
5. Avoid masking the issue by only increasing warehouse size.

## Consultant Recommendation Shape

First prove that pruning is the problem. Clustering is useful when it reduces scan work on important large tables, but it adds maintenance cost.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/02 Performance and Optimization/08 Search Optimization Service]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Bigger Warehouse vs Clustering]]

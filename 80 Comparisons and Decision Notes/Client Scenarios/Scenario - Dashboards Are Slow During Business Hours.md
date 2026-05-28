# Scenario - Dashboards Are Slow During Business Hours

> Client says: "Dashboards are fine early, but slow when everyone logs in."

## Likely Reasoning Path

1. Check whether queries are slow individually or waiting in the warehouse queue.
2. If queueing is the issue, consider BI warehouse multi-cluster with sensible max cluster limits.
3. If individual queries scan too much data, inspect pruning, filters, joins, and query profile.
4. Separate BI from ETL if dashboard users compete with scheduled jobs.
5. Add cost guardrails with auto-suspend, ownership, and monitors.

## Consultant Recommendation Shape

Do not recommend "make the warehouse bigger" until the bottleneck is clear. Concurrency pressure points to scale-out; inefficient scans point to query/model/pruning work.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Virtual Warehouse Size vs Multi-cluster]]
- [[80 Comparisons and Decision Notes/Comparisons/Bigger Warehouse vs Clustering]]
- [[80 Comparisons and Decision Notes/Decision Notes/Choosing a Warehouse Strategy by Workload Type]]

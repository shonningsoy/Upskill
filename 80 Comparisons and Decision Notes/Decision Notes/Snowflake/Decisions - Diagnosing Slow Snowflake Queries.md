---
tags:
  - note-decision
---

# Decisions - Diagnosing Slow Snowflake Queries

> Start with the bottleneck, then choose the lever.

## Decision Frame

A slow Snowflake query is not automatically a warehouse-sizing problem. Use Query History and Query Profile to determine whether the main issue is queueing, scanning, pruning, joins, aggregation/sorting, spilling, or repeated workload design.

## Recommendation Table

| Diagnostic signal | First interpretation | First moves | Escalation options |
|---|---|---|---|
| Query waited in queue | Warehouse concurrency/capacity issue | Check overlapping workloads and warehouse load | Multi-cluster, workload isolation, resize |
| Large scan / weak pruning | Too much data read | Add selective filters, improve predicate design | Clustering, search optimization, model redesign |
| Join outputs many more rows than inputs | Exploding join or wrong grain | Fix join keys, deduplicate, filter before joining | Redesign model grain or staging logic |
| Heavy aggregate, sort, or window | Too many rows processed late | Reduce rows earlier, pre-aggregate, remove unnecessary `DISTINCT` | Materialized view, dynamic table, or persistent intermediate model |
| Local or remote spill | Intermediate data exceeds comfortable memory | Reduce intermediate rows, batch processing | Larger warehouse if query shape is reasonable |
| Same pattern runs frequently | Repeated cost hotspot | Tune the shared SQL/model once | Materialized view, result caching strategy, search optimization, QAS |

## Questions To Ask

- Is the pain from one query, one dashboard, or a repeated query pattern?
- Did execution time come from waiting, scanning, joining, sorting, spilling, or returning results?
- What business SLA or cost target justifies the tuning work?
- What metric will prove the fix worked: elapsed time, queue time, bytes scanned, spill, or total workload time?

## Related Learning Topics

- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/02 Performance and Optimization/07 Materialized Views]]
- [[01 Snowflake/02 Performance and Optimization/08 Dynamic Tables]]
- [[01 Snowflake/02 Performance and Optimization/09 Search Optimization Service]]
- [[01 Snowflake/02 Performance and Optimization/10 Query Acceleration Service]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Bigger Warehouse vs Clustering]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Search Optimization Service vs Clustering]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Virtual Warehouse Size vs Multi-cluster]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Query Scans Too Much Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Dashboards Are Slow During Business Hours]]

# Comparison - Virtual Warehouse Size vs Multi-cluster

> Scale up for more power per query; scale out for more concurrent queries.

## Short Answer

Use a **larger warehouse size** when one query or job needs more compute power. Use **multi-cluster** when many users or jobs are waiting behind each other.

## Comparison Table

| Dimension | Larger warehouse size | Multi-cluster warehouse |
|---|---|---|
| Primary problem | Slow individual query/job | Query queueing and concurrency spikes |
| Main effect | More compute resources per cluster | More clusters serving simultaneous workload |
| Common use | Heavy ETL, backfills, complex transformations | BI dashboards, many analysts, spiky usage |
| Cost risk | Bigger per-second credit burn | More clusters can run at once |
| Consultant recommendation | Try when the bottleneck is per-query compute | Try when the bottleneck is queueing/concurrency |

## Decision Rules

- If a single complex query is slow and not queued, consider scaling up and then tune SQL/modeling.
- If many queries are waiting, consider scaling out with multi-cluster and clear max cluster caps.
- If BI and ETL interfere with each other, separate warehouses before making one shared warehouse larger.
- Use auto-suspend and monitoring either way; both levers can increase spend.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Dashboards Are Slow During Business Hours]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - One Shared Warehouse Is Causing Conflicts]]

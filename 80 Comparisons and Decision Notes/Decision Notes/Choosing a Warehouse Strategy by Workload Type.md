# Choosing a Warehouse Strategy by Workload Type

> Match compute isolation, size, and cluster count to the workload shape.

## Decision Frame

Warehouse strategy should start with workload shape, not a generic size recommendation. Ask whether the workload is compute-heavy, concurrency-heavy, predictable, or exploratory.

## Recommendation Table

| Workload | Starting recommendation | Why | Watch-outs |
|---|---|---|---|
| BI dashboards with many users | Separate BI warehouse, consider multi-cluster | Protects dashboards from ETL and handles bursts | Set max clusters to control spend |
| Heavy ETL/backfills | Separate ETL warehouse, scale size for job window | More compute per job and clearer ownership | Scale back down after exceptional jobs |
| Analyst exploration | Small sandbox warehouse with auto-suspend | Contains ad-hoc cost and risk | Needs ownership and guardrails |
| Mixed BI + ETL on one warehouse | Split workloads first | Reduces noisy-neighbor issues | More objects to govern and monitor |
| Low usage early adoption | Start small with aggressive auto-suspend | Keeps cost predictable | Iterate using query/usage history |

## Questions To Ask

- Which workloads are queueing because of concurrency?
- Which workloads are slow because each query/job is heavy?
- Which teams need isolation for reliability or chargeback?
- What auto-suspend and resource monitor guardrails exist?

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/06 Cost Management and Operations/31 Warehouse Scheduling and Auto-suspend]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Virtual Warehouse Size vs Multi-cluster]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - One Shared Warehouse Is Causing Conflicts]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Dashboards Are Slow During Business Hours]]

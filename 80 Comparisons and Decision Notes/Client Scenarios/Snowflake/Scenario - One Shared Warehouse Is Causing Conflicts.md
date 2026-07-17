---
tags:
  - note-scenario
---

# Scenario - One Shared Warehouse Is Causing Conflicts

> Client says: "BI users complain whenever ETL jobs run."

## Likely Reasoning Path

1. Identify which workloads share the warehouse and when they overlap.
2. Separate BI and ETL into different warehouses to reduce noisy-neighbor contention.
3. Size ETL for heavy transformations and BI for dashboard concurrency.
4. Use auto-suspend and resource monitors so separate warehouses do not become uncontrolled spend.
5. Add ownership/naming standards so teams know which warehouse belongs to which workload.

## Consultant Recommendation Shape

Workload isolation is often a cleaner first move than creating one large shared warehouse. It improves predictability and makes spend easier to attribute.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/06 Cost Management and Operations/46 Warehouse Scheduling and Auto-suspend]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Virtual Warehouse Size vs Multi-cluster]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Warehouse Strategy by Workload Type]]

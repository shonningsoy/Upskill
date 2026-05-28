---
status: seed
platform: Snowflake
area: Core Architecture and Concepts
topic_number: 01
tags:
  - snowflake
  - sf-core-architecture
  - learning
---

# Virtual Warehouses

> Compute clusters that execute queries independently of storage. Consultant lens: Understand sizing, scaling policies, multi-cluster warehouses, and suspension to control cost and performance.

## Executive Summary

- **What it is:** Compute clusters that execute queries independently of storage.
- **Why it matters:** Warehouse design is one of the biggest drivers of Snowflake performance, concurrency, and spend.
- **Mental model:** Warehouses are rented query workers; size affects per-query power, cluster count affects concurrency, and running time affects cost.
- **Best used when:** You need isolated, right-sized compute for BI, ETL, ad-hoc analysis, or mixed workloads.
- **Avoid or reconsider when:** You are trying to solve modeling/query design issues only by scaling compute.

## What It Can Do

- Run SQL queries, ELT jobs, and data loading/unloading workloads.
- Scale up warehouse size to improve performance for heavier queries.
- Scale out with multi-cluster settings to handle concurrency spikes.
- Auto-suspend when idle and auto-resume on demand.
- Isolate workloads by assigning different teams/use cases to separate warehouses.

## What It Cannot Do

- Store data (storage is separate in Snowflake architecture).
- Automatically fix poor SQL, bad joins, or weak data modeling choices.
- Replace governance controls such as RBAC, masking, and policy design.
- Eliminate all bottlenecks through size alone.

## Core Concepts

| Concept | Meaning | Why it matters |
|---|---|---|
| Warehouse size | Vertical scaling of compute resources per cluster. | Impacts performance for heavy single-query workloads. |
| Cluster count | Horizontal scaling via multiple clusters. | Improves concurrency and reduces queueing during spikes. |
| Auto-suspend/auto-resume | Pause/resume compute automatically based on activity. | High-leverage control for reducing idle credit burn. |
| Workload isolation | Separate warehouses for BI, ETL, ad-hoc, etc. | Prevents noisy-neighbor contention and improves predictability. |
| Scaling policy | Rules for adding/removing clusters in multi-cluster mode. | Balances responsiveness against credit consumption. |

## How It Works

A query is assigned to a warehouse. If the warehouse is suspended, Snowflake resumes it, executes the query against shared storage, and then suspends again after an idle threshold. In single-cluster mode, concurrency pressure creates queueing sooner. In multi-cluster mode, Snowflake can add clusters (within min/max limits) to absorb bursts.

## Visuals

![[00 Home/assets/snowflake-core-01-virtual-warehouse-architecture.png]]

- Original local diagram based on Snowflake's virtual warehouse documentation.

## Readable Snippets

```sql
-- BI warehouse with auto-suspend and controlled multi-cluster scaling
create warehouse BI_WH
  warehouse_size = 'MEDIUM'
  auto_suspend = 60
  auto_resume = true
  min_cluster_count = 1
  max_cluster_count = 3
  scaling_policy = 'STANDARD';
```

```sql
-- Isolate ETL from BI to reduce noisy-neighbor issues
create warehouse ETL_WH
  warehouse_size = 'LARGE'
  auto_suspend = 300
  auto_resume = true;
```

```sql
-- Temporarily scale for a heavy backfill, then scale back down
alter warehouse ETL_WH set warehouse_size = 'XLARGE';
-- run backfill
alter warehouse ETL_WH set warehouse_size = 'LARGE';
```

## Consultant Talking Points

- **Client question this answers:** How should we size and separate compute to balance dashboard performance and spend?
- **Trade-offs to mention:** Bigger sizes improve per-query speed; more clusters improve concurrency; both can raise credit consumption.
- **Risk or governance angle:** Lack of ownership and standards can lead to uncontrolled warehouse sprawl and unclear accountability.
- **Cost/performance angle:** Auto-suspend, cluster caps, and workload isolation usually deliver better cost/performance than one shared mega-warehouse.

## Common Pitfalls

- Leaving warehouses running during nights/weekends.
- Sharing one warehouse across all workloads and teams.
- Solving all queueing by only increasing size.
- Ignoring concurrency patterns when setting cluster limits.
- Missing monitors/alerts, causing spend surprises.

## When to Recommend What (Decision Table)

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Small team, light usage, low concurrency | Single-cluster Small/Medium warehouse | Simple baseline with low operational overhead | Can queue during peak windows |
| Spiky BI dashboards with many simultaneous users | Multi-cluster warehouse (min 1, max 2-4) | Better handles burst concurrency | Credit usage can jump if caps are loose |
| Heavy transforms/backfills | Larger single-cluster first | More compute per query/job | Scale back down after workload completes |
| BI and ETL share one warehouse and conflict | Separate BI and ETL warehouses | Workload isolation improves predictability | Slightly more administration |
| Early Snowflake adoption with strict cost goals | Start small + aggressive auto-suspend | Prevents idle spend and surprises | May need iterative right-sizing |
| Query queueing but simple per-query logic | Scale out clusters before scaling up size | Concurrency bottleneck, not per-query bottleneck | Misdiagnosis can waste credits |
| Single complex query runs slowly | Scale up size first, then tune SQL/model | More per-query resources can help | If still slow, likely design/pruning issue |
| Unpredictable analyst exploration | Separate sandbox warehouse with guardrails | Contains ad-hoc cost/risk | Needs ownership and monitoring standards |

## Related Topics

- [[01 Snowflake/01 Core Architecture and Concepts/Core Architecture and Concepts Overview]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/31 Warehouse Scheduling and Auto-suspend]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]

## Related Decision and Scenario Notes

- [[80 Comparisons and Decision Notes/Comparisons/Virtual Warehouse Size vs Multi-cluster]]
- [[80 Comparisons and Decision Notes/Decision Notes/Choosing a Warehouse Strategy by Workload Type]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - One Shared Warehouse Is Causing Conflicts]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Dashboards Are Slow During Business Hours]]

## Questions

- How should we map warehouse strategy to environment tiers (dev/test/prod) for this client?
- Which workloads are concurrency-heavy vs compute-heavy in current usage telemetry?

## Sources To Revisit

- Snowflake docs: Virtual Warehouses - https://docs.snowflake.com/en/user-guide/warehouses
- Snowflake docs: Multi-cluster warehouses and scaling policy - https://docs.snowflake.com/en/user-guide/warehouses-multicluster
- Snowflake docs: Cost and billing for warehouses - https://docs.snowflake.com/en/user-guide/cost-understanding-compute

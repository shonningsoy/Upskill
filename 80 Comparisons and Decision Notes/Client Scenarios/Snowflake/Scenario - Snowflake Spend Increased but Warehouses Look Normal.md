---
tags:
  - note-scenario
---

# Scenario - Snowflake Spend Increased but Warehouses Look Normal

> Client says: "Our Snowflake spend increased, but the warehouses do not look unusually busy."

## Likely Reasoning Path

1. Confirm whether the increase is compute, storage, or data transfer.
2. If compute increased, separate virtual warehouse usage from serverless services, cloud services, Cortex/AI, and compute-pool usage.
3. Check whether consumed cloud services credits were actually billed after the daily adjustment.
4. Look for new or expanded serverless features such as Snowpipe, Automatic Clustering, Search Optimization, Materialized Views, Query Acceleration, serverless tasks, or Cortex usage.
5. Check notebooks, model serving, and other SPCS workloads for compute-pool activity.
6. Review storage retention, staged files, Time Travel, Fail-safe, clone churn, and replication if storage increased.
7. Review cross-region or cross-cloud movement if data transfer increased.
8. Add attribution controls: query tags, warehouse ownership, budgets, service-level monitoring, and naming conventions.

## Consultant Recommendation Shape

Do not stop at warehouse dashboards. A suspended warehouse does not mean Snowflake stopped costing money. Diagnose by cost surface, then by service type, then by owner/workload.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Warehouse usage is normal but total credits rose | Inspect service types and serverless usage |
| Cloud services usage looks high | Check high-frequency metadata, DDL, file listing, and generated-query patterns |
| AI/Cortex usage rose | Attribute by feature, user, workload, and business case |
| Notebooks or endpoints were introduced | Check compute-pool/SPCS usage and idle settings |
| Storage rose | Review retention, stages, history, clones, and churn |
| Ownership is unclear | Add query tags, budgets, named warehouses, and service ownership |

## Watch-outs

- Resource monitors do not cover every Snowflake cost surface.
- Serverless features can be valuable, but their maintenance cost needs review after enabling.
- Consumed cloud services credits are not always fully billed.
- AI and notebook experimentation can become material spend if teams scale before monitoring.
- Storage increases often come from retained history, not only visible tables.
- Without tags and ownership, the conversation becomes blame archaeology. Nobody enjoys blame archaeology.

## Related Learning Topics

- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/45 Account Usage Views]]
- [[01 Snowflake/06 Cost Management and Operations/47 Budgets]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/05 Advanced Analytics and AI/36 Snowflake Notebooks]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Resource Monitors vs Budgets]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Warehouse Strategy by Workload Type]]

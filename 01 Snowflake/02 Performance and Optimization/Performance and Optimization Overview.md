---
status: hub
platform: Snowflake
area: Performance and Optimization
tags:
  - snowflake
  - sf-performance
  - map
---

# Performance and Optimization Overview

> [!abstract] Chapter outcome
> Diagnose the real bottleneck first, then choose the narrowest optimization that improves the workload without creating unjustified maintenance or serverless cost.

## Topics

- [[01 Snowflake/02 Performance and Optimization/06 Query Profile|06 - Query Profile]]
- [[01 Snowflake/02 Performance and Optimization/07 Materialized Views|07 - Materialized Views]]
- [[01 Snowflake/02 Performance and Optimization/08 Dynamic Tables|08 - Dynamic Tables]]
- [[01 Snowflake/02 Performance and Optimization/09 Search Optimization Service|09 - Search Optimization Service]]
- [[01 Snowflake/02 Performance and Optimization/10 Query Acceleration Service|10 - Query Acceleration Service]]
- [[01 Snowflake/02 Performance and Optimization/11 Result Caching|11 - Result Caching]]

## Chapter Map

```mermaid
flowchart TD
    A["Slow or costly query"] --> B["Query Profile"]
    B --> C{"Dominant pattern?"}
    C -->|Repeated computation| D["Materialized View"]
    C -->|Maintained pipeline| E["Dynamic Table"]
    C -->|Selective lookup| F["Search Optimization"]
    C -->|Scan-heavy outlier| G["Query Acceleration"]
    C -->|Identical stable query| H["Result Cache"]

    classDef input fill:#E8F0FE,stroke:#4C6EF5,color:#172B4D
    classDef control fill:#FFF3BF,stroke:#D69E2E,color:#3D2E00
    classDef snowflake fill:#E6FCF5,stroke:#2F9E7B,color:#123C34
    classDef platform fill:#F1F3F5,stroke:#868E96,color:#212529
    classDef output fill:#F3E8FF,stroke:#805AD5,color:#2D1B4E

    class A input
    class B snowflake
    class C control
    class D,E,F,G,H output
```

## Topic Summaries

| Topic | What it unlocks |
|---|---|
| [[01 Snowflake/02 Performance and Optimization/06 Query Profile\|Query Profile]] | Turn a vague performance complaint into evidence: queueing, scans, joins, spill, sorts, or repeated work. Start here. |
| [[01 Snowflake/02 Performance and Optimization/07 Materialized Views\|Materialized Views]] | Trade background maintenance and storage for faster repeated reads of a supported query pattern. |
| [[01 Snowflake/02 Performance and Optimization/08 Dynamic Tables\|Dynamic Tables]] | Manage derived data as a declarative pipeline with freshness targets, not merely as a single-query accelerator. |
| [[01 Snowflake/02 Performance and Optimization/09 Search Optimization Service\|Search Optimization Service]] | Accelerate highly selective lookups on very large tables when ordinary pruning is insufficient. |
| [[01 Snowflake/02 Performance and Optimization/10 Query Acceleration Service\|Query Acceleration Service]] | Add temporary serverless scan capacity for unpredictable eligible outliers without permanently upsizing the warehouse. |
| [[01 Snowflake/02 Performance and Optimization/11 Result Caching\|Result Caching]] | Reuse eligible identical results on unchanged data with no warehouse compute. Treat it as a benefit, not a tuning strategy. |

## How To Use This Area

Begin with [[01 Snowflake/02 Performance and Optimization/06 Query Profile|Query Profile]]. Use evidence from the query plan and history to select a focused optimization; do not start by enabling a service or resizing compute.

## Related Areas

- [[00 Home/Snowflake Learning Map|Snowflake Learning Map]]
- [[01 Snowflake/01 Core Architecture and Concepts/Core Architecture and Concepts Overview|Core Architecture and Concepts]]
- [[01 Snowflake/04 Data Engineering/Data Engineering Overview|Data Engineering]]
- [[01 Snowflake/06 Cost Management and Operations/Cost Management and Operations Overview|Cost Management and Operations]]

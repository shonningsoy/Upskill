---
status: hub
platform: dbt
area: Incremental Processing and Performance
tags:
  - dbt
  - dbt-performance
  - map
---

# Incremental Processing and Performance Overview

> How dbt models are materialized, selected, tuned, rebuilt, and operated efficiently on large Snowflake workloads.

## Topics

- [[02 dbt/04 Incremental Processing and Performance/30 Materializations|30 - Materializations]]
- [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys|31 - Incremental Models and Unique Keys]]
- [[02 dbt/04 Incremental Processing and Performance/32 Incremental Strategies|32 - Incremental Strategies]]
- [[02 dbt/04 Incremental Processing and Performance/33 Microbatch Incremental Models|33 - Microbatch Incremental Models]]
- [[02 dbt/04 Incremental Processing and Performance/34 Parallel Microbatch Execution|34 - Parallel Microbatch Execution]]
- [[02 dbt/04 Incremental Processing and Performance/35 Snapshots vs Incremental Models vs Dynamic Tables|35 - Snapshots vs Incremental Models vs Dynamic Tables]]
- [[02 dbt/04 Incremental Processing and Performance/36 Model Selection State and Deferral|36 - Model Selection, State, and Deferral]]
- [[02 dbt/04 Incremental Processing and Performance/37 Threads Warehouse Sizing and Snowflake Cost|37 - Threads, Warehouse Sizing, and Snowflake Cost]]
- [[02 dbt/04 Incremental Processing and Performance/38 Query Tuning Feedback Loop|38 - Query Tuning Feedback Loop]]
- [[02 dbt/04 Incremental Processing and Performance/39 Full Refreshes Backfills and Replay|39 - Full Refreshes, Backfills, and Replay]]

## Topic Summaries

### [[02 dbt/04 Incremental Processing and Performance/30 Materializations|30 - Materializations]]

Explains views, tables, incremental models, ephemeral models, materialized views, and when each fits.

### [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys|31 - Incremental Models and Unique Keys]]

Core performance pattern for large facts and event streams.

### [[02 dbt/04 Incremental Processing and Performance/32 Incremental Strategies|32 - Incremental Strategies]]

Covers append, merge, delete+insert, insert overwrite, and adapter-specific behavior.

### [[02 dbt/04 Incremental Processing and Performance/33 Microbatch Incremental Models|33 - Microbatch Incremental Models]]

Important for large time-series workloads such as trades, prices, risk snapshots, and intraday events.

### [[02 dbt/04 Incremental Processing and Performance/34 Parallel Microbatch Execution|34 - Parallel Microbatch Execution]]

Speeds up batch windows but needs careful warehouse and dependency design.

### [[02 dbt/04 Incremental Processing and Performance/35 Snapshots vs Incremental Models vs Dynamic Tables|35 - Snapshots vs Incremental Models vs Dynamic Tables]]

Useful cross-tool decision area for history, freshness, and cost.

### [[02 dbt/04 Incremental Processing and Performance/36 Model Selection State and Deferral|36 - Model Selection, State, and Deferral]]

Makes CI and selective production builds practical in large projects.

### [[02 dbt/04 Incremental Processing and Performance/37 Threads Warehouse Sizing and Snowflake Cost|37 - Threads, Warehouse Sizing, and Snowflake Cost]]

Connects dbt concurrency to Snowflake queueing, credit use, and workload isolation.

### [[02 dbt/04 Incremental Processing and Performance/38 Query Tuning Feedback Loop|38 - Query Tuning Feedback Loop]]

Teaches when to change dbt SQL, model shape, materialization, clustering, or warehouse strategy.

### [[02 dbt/04 Incremental Processing and Performance/39 Full Refreshes Backfills and Replay|39 - Full Refreshes, Backfills, and Replay]]

Required for corrections, new logic, historical rebuilds, and disaster recovery.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/03 Testing Documentation and Data Quality/Testing Documentation and Data Quality Overview|Previous: Testing Documentation and Data Quality]]
- [[02 dbt/05 Deployment CI CD and Operations/Deployment CI CD and Operations Overview|Next: Deployment CI CD and Operations]]

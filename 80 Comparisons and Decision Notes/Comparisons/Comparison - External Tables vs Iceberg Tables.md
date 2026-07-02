---
tags:
  - note-comparison
---

# Comparison - External Tables vs Iceberg Tables

> Both keep data in the client's own cloud bucket instead of Snowflake-managed storage. External tables are a read-only window onto files; Iceberg tables are open-format, writable, ACID tables that other engines can share.

## Short Answer

Use **external tables** when the client just needs to query files that already sit in their bucket, reads are occasional, and nothing writes to them. Use **Iceberg tables** when the lake must stay the source of truth *and* needs writes, ACID, time travel, near-internal performance, or sharing with other engines (Spark, Trino, Databricks).

## Comparison Table

| Dimension | External tables | Iceberg tables |
|---|---|---|
| Primary purpose | Read files in place | Manage open-format lake tables in place |
| Storage owner | Client's bucket | Client's bucket |
| Read/write | **Read-only** | **Read + write (ACID)** with Snowflake-managed catalog |
| Format | Parquet / JSON / CSV files | Apache Iceberg (open) |
| Time travel / snapshots | No | Yes (Iceberg snapshots) |
| Schema evolution | Limited | Yes |
| Performance | Slow; needs partitioning + materialized views | Near-internal (Snowflake-managed catalog) |
| Multi-engine sharing | No | Yes (open format) |
| Governance in Snowflake | Basic | Full when Snowflake owns the catalog; limited with external catalog |
| Cost considerations | Cheap setup, scan-heavy queries | No second storage copy, but still pay SF compute + setup |
| Consultant recommendation | Occasional, read-only lake queries | Modern default when the lake is source of truth |

## Decision Rules

- If the workload is **read-only and occasional**, external tables are the simplest, cheapest option.
- If the client needs **writes, ACID, or time travel** on lake data, that rules out external tables — use Iceberg.
- For Iceberg, **whoever owns the catalog is the writer**: Snowflake-managed = Snowflake can write (best perf + governance); external catalog (Glue/REST) = Snowflake is read-only.
- If **avoiding lock-in or sharing with Spark/Trino** matters, Iceberg's open format is the reason to choose it.
- If Snowflake is simply the hub and performance/simplicity win, neither — load into **internal tables**.
- Don't forget performance tuning: external tables scan everything without partitioning; small files hurt both.

## Related Learning Topics

- [[01 Snowflake/04 Data Engineering/24 External Tables and Iceberg]]
- [[01 Snowflake/04 Data Engineering/19 Stages and Data Loading]]
- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/02 Performance and Optimization/07 Materialized Views]]

## Related Scenarios

- 

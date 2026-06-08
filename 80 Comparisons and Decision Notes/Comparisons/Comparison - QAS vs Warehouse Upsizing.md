---
tags:
  - note-comparison
---

# Comparison - QAS vs Warehouse Upsizing

## Short Answer

Use **QAS** when occasional scan-heavy outlier queries slow down a warehouse that's otherwise right-sized.
Use **Warehouse Upsizing** when most queries consistently need more compute (joins, sorts, aggregations).

## Comparison Table

| Dimension | Query Acceleration Service (QAS) | Warehouse Upsizing |
|---|---|---|
| Primary purpose | Burst compute for scan/filter portions of outlier queries | Permanently increase compute for all queries |
| Strengths | Elastic, no idle cost, surgical (targets only eligible scans) | Benefits all query types (joins, sorts, aggregation), predictable |
| Limits | Only helps scan-bound queries; doesn't help joins/sorts | Over-provisions for the 95% of queries that don't need it |
| Cost model | Serverless credits — pay per use, no idle cost | Warehouse credits — pay for every second it runs, including idle |
| Cost risk | Surprise costs if many queries qualify and scale factor is high | Predictable but wasteful; easy to forget to downsize |
| Governance considerations | Monitor with `SYSTEM$ESTIMATE_QUERY_ACCELERATION`; cap with scale factor | Monitor with Resource Monitors; schedule auto-suspend |
| Best workload fit | Ad-hoc analyst exploration, mixed workloads with rare outliers | Consistent heavy compute (ETL transforms, complex joins) |
| Consultant recommendation | Start here for unpredictable workloads; prove value before committing | Use when most queries need the power, not just occasional ones |

## Decision Rules

- If the slow queries are scan-heavy with selective filters on large tables → try QAS first.
- If the slow queries are join/sort/aggregation-bound → QAS won't help; consider upsizing.
- If only 5-10% of queries need extra power → QAS is more cost-efficient than upsizing for 100% of runtime.
- If most queries consistently under-perform → upsizing gives universal lift.
- If cost predictability matters most → upsizing is simpler to budget; QAS needs monitoring.
- When unsure → run `SYSTEM$ESTIMATE_QUERY_ACCELERATION` on the top 10 slow queries to decide.

## Related Learning Topics

- [[01 Snowflake/02 Performance and Optimization/10 Query Acceleration Service]]
- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/02 Performance and Optimization/06 Query Profile]]
- [[01 Snowflake/06 Cost Management and Operations/30 Credit Consumption Model]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Query Scans Too Much Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Dashboards Are Slow During Business Hours]]

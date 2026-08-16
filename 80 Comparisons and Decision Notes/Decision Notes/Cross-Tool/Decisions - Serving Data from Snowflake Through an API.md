---
tags:
  - note-decision
---

# Decisions - Serving Data from Snowflake Through an API

> Decide when a bounded API may query curated Snowflake data directly and when caching, asynchronous delivery, or an operational serving store is required.

## Decision Frame

Start with consumer latency, freshness, concurrency, result size, authorization, and query variability. Snowflake can be the governed analytical source without always being the correct request-time engine.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Low-volume bounded analytical lookup | API over curated Snowflake serving model | Simple, governed, and sufficiently fresh | Warehouse startup, query limits, role isolation, cost attribution |
| Repeated response tolerates short staleness | Authorization-aware cache in front of Snowflake | Reduces latency and repeated compute | Tenant-safe keys, invalidation, encryption, and retention |
| Large or slow analytical export | Async job and result/file retrieval | Avoids fragile long-held HTTP requests | Job authorization, expiry, cleanup, and retries |
| High-QPS millisecond point reads | Operational serving store fed by governed pipeline | Matches operational latency and concurrency | Added synchronization, platform, and consistency model |
| Consumers need broad analytical exploration | BI, semantic layer, share, or governed query access | API endpoints would become a poor ad hoc query language | Entitlements, workload isolation, and contract fit |
| Team wants runtime close to Snowflake | Evaluate SPCS against external managed containers | May simplify data locality and Snowflake identity | Compute pools, platform skills, region limits, portability |

## Questions To Ask

- What are the p95/p99 latency and availability expectations?
- How stale may a response be, and how is freshness communicated?
- What are peak request concurrency, page size, and query variability?
- Does each caller have different row- or object-level entitlements?
- Can the business logic be precomputed in dbt or Snowflake?
- Which team owns the API, warehouse, cache/store, and incident path?
- What condition would trigger a move away from direct Snowflake serving?

## Related Learning Topics

- [[05 APIs/03 Designing and Building APIs/16 Connecting an API to Snowflake|Connecting an API to Snowflake]]
- [[05 APIs/04 Production Security and Data Stack Integration/20 API Security and Authorization|API Security and Authorization]]
- [[05 APIs/04 Production Security and Data Stack Integration/23 Performance and Cost Boundaries|Performance and Cost Boundaries]]
- [[05 APIs/04 Production Security and Data Stack Integration/24 Snowflake API and Integration Surfaces|Snowflake API and Integration Surfaces]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Snowflake Python Connector vs SQL API|Snowflake Python Connector vs SQL API]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Warehouse Inference vs SPCS Model Serving|Warehouse Inference vs SPCS Model Serving]]

## Sources To Revisit

- [Snowflake Docs: Warehouses Overview](https://docs.snowflake.com/en/user-guide/warehouses-overview)
- [Snowflake Docs: Snowpark Container Services Overview](https://docs.snowflake.com/en/developer-guide/snowpark-container-services/overview)
- [Snowflake Docs: Snowflake SQL API](https://docs.snowflake.com/en/developer-guide/sql-api/index)

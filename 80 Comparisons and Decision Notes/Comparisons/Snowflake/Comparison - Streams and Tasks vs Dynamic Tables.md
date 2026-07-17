---
tags:
  - note-comparison
---

# Comparison - Streams and Tasks vs Dynamic Tables

## Short Answer

Use **Dynamic Tables** when the transformation is a declarative `SELECT` (filters, projections, joins, aggregations), the data is small-to-moderate or reliably qualifies for incremental refresh, and you want Snowflake to manage freshness and the refresh DAG with minimal code.

Use **Streams + Tasks** when you need procedural control, your logic risks forcing a **full refresh** (e.g. window-function dedup), you want **append-only / as-of** semantics, or the base table is so large a full refresh would be ruinous.

## Comparison Table

| Dimension | Streams + Tasks | Dynamic Tables |
|---|---|---|
| Primary purpose | Imperative incremental pipelines — you write the *how* | Declarative maintained transforms — you write the *what* |
| You manage | Stream, schedule, MERGE/insert logic, DAG (`AFTER`) | Just the `SELECT` + `TARGET_LAG` |
| Refresh model | You control exactly what runs and when | Snowflake refreshes to meet `TARGET_LAG`; incremental or **full** |
| Joins | Yes, you code them (as-of enrichment at load time) | Yes, but dim changes can retroactively rewrite history |
| Window-function dedup | Natural fit via `QUALIFY` + `MERGE` | Tends to **disqualify incremental** → full refresh risk |
| Scale behaviour | Process-once on the delta; never rescans the base | Incremental usually; full refresh recomputes everything |
| Cost considerations | Pay per task run; gate with `SYSTEM$STREAM_HAS_DATA` | `TARGET_LAG` is the cost dial; full-refresh fallback is the budget killer |
| Governance considerations | More objects/failure points; full control + auditability | Fewer objects, simpler to reason about; watch silent full refreshes |
| Consultant recommendation | Large/append-only, dedup, procedural, fine control | Declarative, chained models, modest scale, less code |

## Decision Rules

- **Default to Dynamic Tables** for straightforward declarative transforms and chained multi-layer models (use `TARGET_LAG = DOWNSTREAM` for lazy intermediates).
- **Switch to Streams + Tasks** the moment you hit: window-function dedup, billions of rows, append-only/as-of needs, or procedural logic (stored procs, multi-step).
- **At extreme scale (e.g. 60B rows), avoid anything that can full-refresh.** Streams + Tasks physically processes only the new delta; a Dynamic Table's full-refresh fallback is the tail risk to fear.
- **Join semantics matter:** Streams + Tasks enrich at load time and don't rewrite history when a dimension changes (as-of). Dynamic Tables keep the result current and may retroactively rewrite historical rows — expensive and often not what you want for event data.
- **Dedup pattern:** collapse the batch to one row per key with `QUALIFY ROW_NUMBER() = 1`, then `MERGE` on the key so late/cross-batch duplicates are absorbed against the target. Dedup the source **before** the MERGE or Snowflake raises *"Duplicate row detected during DML action"* (non-deterministic match protection).

## Related Learning Topics

- [[01 Snowflake/04 Data Engineering/20 Streams and Tasks]]
- [[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
- [[01 Snowflake/04 Data Engineering/22 Snowpipe]]
- [[01 Snowflake/02 Performance and Optimization/07 Materialized Views]]

## Related Scenarios

- 

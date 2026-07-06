---
tags:
  - note-scenario
---

# Scenario - Duplicate Trade Event Arrives Repeatedly

> Client says: "A source-system bug sometimes sends the same trade event in different files over several days. We cannot fix the source, but positions must never double-count it."

## Likely Reasoning Path

1. Confirm the feed is event-based rather than a sequence of valid daily snapshots.
2. Separate the identifiers: `event_id` identifies one delivered business event; `execution_id` identifies the underlying trade execution.
3. Preserve every delivered observation in raw with source filename, source row number, and load timestamp.
4. Collapse multiple copies of the same event within each incoming batch before `MERGE` so the source contains one row per merge key.
5. Merge into a persistent canonical-event table by `event_id`, allowing duplicates arriving days later to match an already accepted event.
6. Treat the first valid identical payload as canonical; update only `last_seen_at`, source-observation metadata, and occurrence count for later identical deliveries.
7. Apply corrections and cancellations as separate, versioned events when building current trade state.
8. Quarantine and alert when the same `event_id` arrives with a different payload; do not silently choose first or latest.

## Consultant Recommendation Shape

Design for at-least-once delivery and make the downstream effect idempotent. After five identical deliveries, raw should contain five observations, the canonical ledger should contain one event with `occurrence_count = 5`, and current trade state should contain one financial effect. Use source event version and event time for business ordering; use `loaded_at` for latency and investigation, not to determine trade truth.

## Related Learning Topics

- [[01 Snowflake/04 Data Engineering/24.5 Bonus chapter Data from A-Z]]
- [[01 Snowflake/04 Data Engineering/20 Streams and Tasks]]
- [[01 Snowflake/04 Data Engineering/22 Snowpipe]]
- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Streams and Tasks vs Dynamic Tables]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Snowflake Ingestion Method]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Modeled Historical Data]]

## Questions To Ask

- Is `event_id` globally unique and immutable, and can it ever be reused by the source?
- Does a legitimate correction receive a new event ID and version while retaining the same execution ID?
- Is the payload byte-for-byte stable, or must it be normalized before hashing?
- Which duplicate and conflict rates should trigger operational alerts or reconciliation failure?
- How long must every raw delivery observation be retained for audit and investigation?

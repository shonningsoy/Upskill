---
tags:
  - note-comparison
---

# Comparison - Snapshots vs Incremental Models

> Incremental models optimize how much data dbt processes. Snapshots create observed history when a source system overwrites records.

## Short Answer

Choose an **incremental model** when the source already contains the rows you need and the main problem is processing only the new or changed subset efficiently.

Choose a **snapshot** when the source only shows the current state, old values disappear, and the business needs to ask what a record looked like at an earlier point in time.

The decisive question is:

```text
Does the source already store history, or does dbt need to create history by observing changes?
```

## Comparison Table

| Dimension | Incremental model | Snapshot |
|---|---|---|
| Primary job | Efficiently update a model without full rebuilds | Preserve historical versions of mutable records |
| Best source shape | Append-only events, CDC logs, dated facts, or already-historical tables | Current-state tables that overwrite old values |
| Output shape | Whatever the model SQL defines | SCD2-style rows with validity columns |
| Typical metadata | `unique_key`, incremental filter, merge/update strategy | `unique_key`, `strategy`, `updated_at` or `check_cols`, `dbt_valid_from`, `dbt_valid_to` |
| Captures every change? | Yes if the source contains every change and the model handles late/corrected data | No, only states observed when the snapshot runs |
| Main risk | Missing late-arriving or corrected records | Missing intermediate changes between snapshot runs |
| Main cost driver | Reprocessing window, merge size, downstream joins | Source scan size, change detection, run cadence, history growth |
| Common use | Fact tables, event history, rolling append data | Customer/account status, risk rating, owner, address, product attributes |

## Decision Rules

- If the source has one row per event, period, or version, use an incremental model.
- If the source overwrites the same business key and old values disappear, use a snapshot.
- If the business needs current state only, a normal table or incremental model is usually simpler.
- If the business needs point-in-time state for mutable dimensions, use snapshots or another explicit SCD2 pattern.
- If the source provides reliable CDC, use incremental processing over the CDC stream instead of snapshotting a huge table.
- If every intra-day change matters, do not rely on a daily snapshot.
- If the table is huge and volatile, validate snapshot cost before recommending it.

## Example

Source already has history:

```text
cust_id | value | effective_date
1       | 1     | 2026-01-01
1       | 2     | 2026-02-01
1       | 3     | 2026-03-01
```

Use an incremental model to load the new dated rows.

Source only has current state:

```text
cust_id | risk_rating
1       | high
```

If yesterday the value was `medium`, the source no longer shows it. Use a snapshot to preserve the old version when dbt observes the change.

## Consultant Recommendation Shape

> "Incremental models are a performance and processing pattern. Snapshots are a history-capture pattern. If the source already gives us history, incremental usually wins. If the source overwrites history, snapshots give us a controlled way to preserve observed versions."

## Watch-outs

- Do not sell snapshots as complete audit logs unless the run cadence, source timing, and controls support that claim.
- Do not use a naive incremental filter such as `where date > max(date)` if late-arriving records are common.
- Do not snapshot every column of a wide volatile table unless the business truly needs that history.
- Do not join snapshots by key alone; align validity windows for point-in-time reporting.
- Do not assume an incremental model preserves history; it preserves whatever the SQL is designed to preserve.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking]]
- [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys]]
- [[02 dbt/04 Incremental Processing and Performance/35 Snapshots vs Incremental Models vs Dynamic Tables]]
- [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Time Travel vs Modeled Historical Data]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Bank Needs to Reconstruct Customer Risk Rating at Report Time]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Daily Load Overwrote Good Data]]

## Sources To Revisit

- [dbt Docs: Snapshots](https://docs.getdbt.com/docs/build/snapshots)
- [dbt Docs: Incremental models](https://docs.getdbt.com/docs/build/incremental-models)
- [dbt Docs: Incremental strategy](https://docs.getdbt.com/docs/build/incremental-strategy)

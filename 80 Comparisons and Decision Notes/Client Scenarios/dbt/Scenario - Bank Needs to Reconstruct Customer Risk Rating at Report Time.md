---
tags:
  - note-scenario
---

# Scenario - Bank Needs to Reconstruct Customer Risk Rating at Report Time

> Client says: "For a regulatory review, we need to prove what each customer's risk rating was when the monthly report was produced."

## Likely Reasoning Path

1. Confirm whether the source system stores full risk-rating history or only the latest value.
2. If the source already has dated history, model it with an incremental or table model.
3. If the source overwrites current state, use a snapshot to preserve observed changes.
4. Confirm the unique key, update timestamp, and columns that define a meaningful risk-rating change.
5. Decide whether snapshot observation time is good enough or whether legal business-effective dates are required.
6. Build downstream point-in-time logic that joins facts to the correct risk-rating version.
7. Test validity windows, current rows, duplicate keys, and expected change behavior.
8. Document the limitation that snapshots observe states on their run schedule.

## Consultant Recommendation Shape

Do not default to incremental just because the table is large. If the source overwrites customer risk ratings, the core requirement is historical reconstruction. Use a snapshot or another explicit history-capture pattern, then build tested point-in-time models for reporting.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Source has complete risk-rating history | Incremental historical model |
| Source only has latest risk rating | dbt snapshot |
| Every intra-day change matters | Upstream CDC or audit log |
| Need month-end reporting state | Snapshot plus point-in-time mart |
| Need legal proof of source-system decision timing | Validate source audit controls, not only dbt history |

## Watch-outs

- Snapshot history is observed history, not necessarily complete legal audit history.
- A daily snapshot can miss changes that occur and revert between runs.
- `updated_at` must change for every relevant business update if using timestamp strategy.
- Wide `check_cols` can become expensive and noisy.
- Point-in-time joins need validity-window logic, not just `customer_id`.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking]]
- [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys]]
- [[02 dbt/03 Testing Documentation and Data Quality/21 Generic Singular and Custom Data Tests]]
- [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products]]
- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Snapshots vs Incremental Models]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Time Travel vs Modeled Historical Data]]

---
tags:
  - note-comparison
---

# Comparison - Full Refresh vs Backfill vs Replay

> Choose the smallest controlled reconstruction that restores correctness across the full downstream impact scope.

## Short Answer

Use a **full refresh** when the entire selected model must be reconstructed and the source still contains everything required.

Use a **backfill** when a known historical period or key range is missing, incorrect, or affected by new logic.

Use a **replay** when past processing must be safely repeatable from controlled inputs, code, and parameters.

`dbt retry` is different: it continues the prior failed invocation rather than deliberately recalculating historical business data.

## Comparison Table

| Dimension | Full refresh | Backfill | Replay |
|---|---|---|---|
| Primary purpose | Reconstruct the whole selected model | Repair a bounded historical scope | Repeat past processing predictably |
| Typical trigger | Global logic or grain change, broad corruption | Missing period, late delivery, localized correction | Recovery drill, reproducibility, controlled recalculation |
| Processing scope | All available history for selected models | Explicit dates, keys, partitions, or batches | Explicit historical inputs and execution context |
| Main strength | Simple complete reset | Lower cost and blast radius | Strong recovery and auditability |
| Main limitation | Potentially expensive and disruptive | Boundary and downstream-impact complexity | Requires retained/versioned inputs and deterministic logic |
| Idempotency requirement | Desirable | Required for safe reruns | Fundamental |
| Source requirement | Complete reconstructable history | Required data for affected scope | Reproducible input version or accepted current-state recalculation |
| Downstream handling | Rebuild affected descendants deliberately | Trace impact beyond correction window | Recreate all outputs governed by the replay contract |
| Cost shape | Broad scan and rebuild | Bounded processing | Depends on retained scope and recovery design |
| Finance control | Approval when published results can change | Period reopening and reconciliation | Code/input evidence, approval, and publication versioning |

## Decision Rules

- Use the smallest scope that can restore complete downstream correctness.
- Choose full refresh when logic changed for all history and reconstruction remains affordable.
- Choose a targeted backfill when the correction scope is known, but calculate the downstream impact scope separately.
- Choose replay when the business must prove that a historical result can be reproduced or recalculated under controlled assumptions.
- Use `dbt retry` only when the previous invocation is still the intended operation and successful results remain valid.
- Do not attempt any option until source history and reference-data versions are confirmed.
- Use inclusive start and exclusive end boundaries with an explicit timezone.
- Make repeated execution safe through merge, deduplication, scoped replacement, or idempotent microbatch batches.

## Correction Scope Is Not Impact Scope

| Corrected logic | Likely impact |
|---|---|
| Independent daily total | Corrected dates |
| Monthly total | Affected month |
| Seven-day rolling metric | Corrected dates plus six later dates |
| Running balance | Correction point through all later balances |
| Customer lifetime metric | Affected customers and downstream aggregates |
| Closed finance publication | Governed restatement or immutable new publication version |

## Related Learning Topics

- [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys]]
- [[02 dbt/04 Incremental Processing and Performance/33 Microbatch Incremental Models]]
- [[02 dbt/04 Incremental Processing and Performance/37 Threads Warehouse Sizing and Snowflake Cost]]
- [[02 dbt/04 Incremental Processing and Performance/39 Full Refreshes Backfills and Replay]]

## Related Decisions and Scenarios

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Incremental Strategy on Snowflake]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a Data History and Restatement Pattern]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Duplicate Trade Event Arrives Repeatedly]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Trade Batch Must Be Validated Before Publication]]

## Sources To Revisit

- [dbt Developer Hub - `full_refresh` configuration](https://docs.getdbt.com/reference/resource-configs/full_refresh)
- [dbt Developer Hub - Microbatch backfills](https://docs.getdbt.com/docs/build/incremental-microbatch)
- [dbt Developer Hub - `dbt retry`](https://docs.getdbt.com/reference/commands/retry)
- [dbt Developer Hub - Run results JSON file](https://docs.getdbt.com/reference/artifacts/run-results-json)

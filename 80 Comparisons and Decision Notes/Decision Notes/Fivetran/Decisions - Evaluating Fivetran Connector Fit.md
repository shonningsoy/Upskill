---
tags:
  - note-decision
---

# Decisions - Evaluating Fivetran Connector Fit

> Decide whether a specific Fivetran connector is suitable for a specific source, dataset, and service expectation.

## Decision Frame

A connector recommendation must be workload-specific. Evaluate exact coverage, row identity, change and delete capture, history, latency, recovery, source impact, schema evolution, security, maturity, cost, and ownership before approving production use.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Mature native connector meets all material requirements | Approve a representative pilot | Strong candidate for managed standardization | Validate difficult behavior, not only initial setup |
| Connector has a non-critical coverage gap | Approve with documented workaround and owner | Managed value may still exceed the exception cost | Workaround must be tested and supported |
| Lite or preview connector covers a material process | Pilot with explicit maturity risk | Can accelerate access to a new source | Coverage, release change, and support expectations |
| Unsupported stable private API | Evaluate Connector SDK or custom ingestion | Enables source-specific extraction | Client owns code, tests, releases, and support |
| Critical key, delete, history, security, or recovery need is unmet | Reject for that workload | Correctness or control gate failed | Reassess only with new evidence |

## Questions To Ask

- Which exact objects, fields, accounts, and historical periods are required?
- How are inserts, updates, deletes, and schema changes exposed by the source?
- Are primary keys stable, and what happens when they are missing or change?
- What end-to-end freshness and recovery objectives apply?
- Which source permissions, logs, quotas, retention, network path, and plan features are required?
- Who owns monitoring, reconciliation, schema review, incidents, and connector change?

## Related Learning Topics

- [[03 Fivetran/01 Foundations and Platform Mental Model/05 When to Recommend Fivetran]]
- [[03 Fivetran/02 Connectors and Sync Behavior/06 Connector Types Coverage and Maturity]]
- [[03 Fivetran/02 Connectors and Sync Behavior/10 Initial Incremental Re-import and Re-sync Strategies]]
- [[03 Fivetran/02 Connectors and Sync Behavior/11 Scheduling Latency Checkpoints and Recovery]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/32 Fivetran Recommendation and Enterprise Adoption Framework]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - Fivetran vs Custom Ingestion]]

## Sources To Revisit

- [Fivetran Connectors](https://fivetran.com/docs/connectors)
- [Fivetran Core Concepts](https://fivetran.com/docs/core-concepts)
- [Fivetran Connector SDK](https://fivetran.com/docs/connector-sdk)

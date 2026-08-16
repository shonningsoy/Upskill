---
tags:
  - note-decision
---

# Decisions - Build vs Buy API Ingestion

> Decide whether a source API justifies a maintained custom extractor or should be delegated to a managed connector.

## Decision Frame

Compare source coverage and required controls over the full lifecycle: authentication, pagination, incremental state, quotas, schema change, deletes, historical recovery, observability, security review, and on-call ownership.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Mature managed connector covers required objects and history | Buy/use managed connector | Transfers source-specific maintenance and recovery work | Validate field coverage, sync semantics, cost, region, and support |
| Source is strategic but unsupported | Build a focused extractor | Fills a real capability gap | Fund maintenance, testing, monitoring, and source-change response |
| Connector exists but misses one small object | Hybrid: managed connector plus narrow custom extraction | Avoids rebuilding common coverage | Reconcile identities, schedules, schemas, and ownership |
| Requirement is temporary or exploratory | Small time-boxed prototype | Tests access and data value cheaply | Do not mistake the prototype for a production service |
| No owner for credentials, incidents, or replay | Do not build yet | Custom code creates an ongoing service | Establish operating model or select supported product |

## Questions To Ask

- Which endpoints, fields, history, deletes, and freshness are actually required?
- How does the API paginate, rate-limit, expire tokens, and expose incremental changes?
- Can missed history be replayed after a long outage?
- How will landed data be reconciled to the source?
- Who owns source changes, credential rotation, incidents, and Snowflake cost?
- What is the three-year cost of product fees versus custom maintenance?

## Related Learning Topics

- [[05 APIs/02 Consuming APIs for Data Pipelines/10 Pagination Filtering and Incremental Extraction|Pagination, Filtering, and Incremental Extraction]]
- [[05 APIs/02 Consuming APIs for Data Pipelines/13 API Ingestion Correctness|API Ingestion Correctness]]
- [[05 APIs/04 Production Security and Data Stack Integration/25 Consultant API Decision Framework|Consultant API Decision Framework]]
- [[03 Fivetran/02 Connectors and Sync Behavior/07 Application and API Connectors|Fivetran Application and API Connectors]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - Fivetran vs Custom Ingestion|Fivetran vs Custom Ingestion]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - API vs File vs CDC|API vs File vs CDC]]

## Sources To Revisit

- [Fivetran Docs: Connector Directory](https://fivetran.com/docs/connectors)
- [Fivetran Docs: Core Concepts](https://fivetran.com/docs/core-concepts)

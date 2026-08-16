---
tags:
  - note-comparison
---

# Comparison - API vs File vs CDC

## Short Answer

Use an **API** for targeted request/response access or controlled actions, a **file** for large replayable batches, and **CDC** for ordered database changes with low-latency continuity.

## Comparison Table

| Dimension | API | File | CDC |
|---|---|---|---|
| Primary purpose | Interactive objects, actions, or bounded extraction | Bulk snapshots and scheduled exchange | Continuous database change capture |
| Strengths | Explicit contract, filters, selective access, automation | Efficient large volume, easy retention and replay | Low-latency inserts, updates, deletes, and ordering |
| Limits | Quotas, pagination, history windows, consumer coupling | Higher batch latency and manifest handling | Source-log access, retention, operational complexity |
| Recovery | Checkpoint, retry, dedupe, and sometimes re-fetch | Reload an immutable file or partition | Resume from retained log position or resnapshot |
| Cost considerations | Calls, custom support, source fees, Snowflake loading | Storage, transfer, orchestration, bulk loading | Connector/runtime cost and source/destination impact |
| Governance considerations | Tokens, object authorization, data minimization, versioning | Encryption, manifests, retention, delivery access | Privileged source access, log contents, delete handling |
| Consultant recommendation | Prefer for small interactive contracts | Prefer for large periodic datasets | Prefer for database changes requiring continuity |

## Decision Rules

- Do not call an API "real time" until the source data freshness and delivery latency are known.
- Prefer a bulk endpoint or file over millions of small paginated requests when both are supported.
- Prefer CDC when database change order and deletes matter and the source can retain logs safely.
- Require replay and reconciliation evidence for every option.
- Use a managed connector when it already handles the source-specific mechanics with acceptable control and cost.

## Related Learning Topics

- [[05 APIs/02 Consuming APIs for Data Pipelines/12 Polling Webhooks Async Jobs and Bulk Exports|Polling, Webhooks, Async Jobs, and Bulk Exports]]
- [[05 APIs/02 Consuming APIs for Data Pipelines/13 API Ingestion Correctness|API Ingestion Correctness]]
- [[05 APIs/04 Production Security and Data Stack Integration/25 Consultant API Decision Framework|Consultant API Decision Framework]]
- [[03 Fivetran/02 Connectors and Sync Behavior/08 Database Connectors CDC and High-Volume Agent|Fivetran Database Connectors and CDC]]

## Related Scenarios

- No dedicated scenario yet; use the decision framework and build-versus-buy decision note.

---
tags:
  - note-decision
---

# Decisions - Designing dbt Observability and Incident Evidence

> Decide what evidence a dbt-on-Snowflake operation must retain so failures, freshness, cost, access, and recovery are explainable.

## Decision Frame

Clients often ask, **"How do we monitor dbt?"**

A stronger framing is: **"What must we be able to prove when dbt is late, expensive, failing, or disputed?"**

The answer depends on:

- Which pipelines are business-critical or regulated.
- Whether the team needs runtime monitoring, audit evidence, or both.
- Which dbt artifacts and Snowflake metadata can be retained and queried.
- Whether source freshness, data quality, cost, access, and downstream publication are all in scope.
- Who owns alerts, incident response, rollback, replay, and post-incident review.

The principle is: **store evidence by invocation, correlate dbt to Snowflake, and alert only on signals with owners and runbooks.**

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Team only sees job green/red | Retain `run_results.json`, logs, and job metadata | Gives node-level outcomes and timing | Local `target/` output is not durable history |
| Slim CI is used | Store approved production `manifest.json` by commit | Enables state comparison and deferral | Do not overwrite trusted state with partial runs |
| Source lateness causes incidents | Run freshness checks and retain `sources.json` | Separates upstream readiness from transformation failure | Freshness does not prove completeness |
| Snowflake spend is unclear | Add dbt query tags and monitor warehouse/query attribution views | Connects dbt workloads to credits | Query attribution is not complete invoice attribution |
| Critical models are slow | Track dbt runtime and join to Snowflake query history | Shows both model trend and warehouse execution behavior | Needs correlation strategy |
| Regulated production output | Immutable evidence bundle per production invocation | Supports audit, incident review, and restatement decisions | Control access to query text and compiled SQL |
| Alert fatigue exists | Define severities, owners, thresholds, and runbooks | Makes alerts actionable | Some signals belong in daily review, not paging |
| Sensitive data is involved | Govern artifact, log, query-text, and access-history visibility | Observability data can expose confidential metadata | Redaction and retention rules matter |
| Downstream BI complaints continue | Extend observability beyond dbt into BI refreshes and extracts | dbt success may not mean consumers are updated | Requires cross-tool ownership |
| Early-stage project | Start with artifacts, source freshness, query tags, and a basic dashboard | Delivers value without heavy platform build | Design paths so metadata can mature later |

## Questions To Ask

- Which jobs, models, sources, and reports are critical enough to monitor?
- What evidence is retained for each invocation: Git SHA, command, target, variables, user, role, warehouse, artifacts, logs?
- Where are artifacts stored, and how long are they retained?
- Can dbt models be correlated to Snowflake queries through query tags, query IDs, timing, or invocation metadata?
- Which alerts are urgent and which belong in a daily operating review?
- What does "late", "failed", "stale", "expensive", or "suspicious" mean for each critical data product?
- Who owns each alert and runbook?
- What is the incident path when a job is green but data is wrong?
- Which observability metadata is sensitive and who may query it?
- Does the evidence support rollback, replay, audit, and stakeholder communication?

## Related Learning Topics

- [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results]]
- [[02 dbt/05 Deployment CI CD and Operations/48 Incident Response Rollback and Replay]]
- [[02 dbt/05 Deployment CI CD and Operations/49 Observability with dbt and Snowflake Metadata]]
- [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC]]
- [[02 dbt/03 Testing Documentation and Data Quality/23 Source Freshness and SLA Monitoring]]
- [[02 dbt/03 Testing Documentation and Data Quality/29 Data Quality Strategy in Regulated Environments]]
- [[01 Snowflake/06 Cost Management and Operations/45 Account Usage Views]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Freshness vs Completeness vs Validity vs Reconciliation]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Quality Control]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Diagnosing Snowflake Spend Increases]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Diagnosing Slow Snowflake Queries]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Dashboard Published From Stale Source Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Team Cannot Tell Which Dashboards a Model Change Will Break]]

## Sources To Revisit

- [dbt Developer Hub - About dbt artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts)
- [dbt Developer Hub - Run results JSON file](https://docs.getdbt.com/reference/artifacts/run-results-json)
- [dbt Developer Hub - Sources JSON file](https://docs.getdbt.com/reference/artifacts/sources-json)
- [Snowflake Docs - QUERY_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/query_history)
- [Snowflake Docs - WAREHOUSE_METERING_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_metering_history)
- [Snowflake Docs - ACCESS_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/access_history)

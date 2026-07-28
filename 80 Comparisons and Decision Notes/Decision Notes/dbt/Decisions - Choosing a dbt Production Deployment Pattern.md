---
tags:
  - note-decision
---

# Decisions - Choosing a dbt Production Deployment Pattern

> Choose how approved dbt code becomes trusted production data, with clear identity, timing, evidence, and recovery controls.

## Decision Frame

Clients usually ask, **"How should dbt actually get to production?"**

The answer depends on more than whether the SQL works:

- Does every merge deploy immediately, or does production run on a schedule?
- Is there a staging/UAT step before production?
- Who or what owns the production execution identity?
- What artifacts and logs prove what ran?
- What happens if a deploy succeeds technically but publishes wrong data?
- Which rollback, replay, and communication controls are required?

The principle is: **CI validates proposed code; deployment runs approved code; observability and incident response protect trusted outputs.**

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Small project with low blast radius | Merge to `main`, scheduled deploy job | Simple and easy to operate | Still needs artifacts, alerts, and rollback path |
| Team wants fast continuous delivery | Mandatory CI, protected branch, deploy on merge | Keeps production close to approved code | Requires safe changes, monitoring, rollback/replay, and small PRs |
| Production should update on business cadence | Merge freely after CI; deploy job runs on schedule | Separates code approval from data publication timing | Communicate when merged code becomes live |
| Slim CI needs current production state | Add merge job or manifest-refresh job | Publishes trusted manifest for future state comparison | Do not publish failed or partial manifests as baseline |
| Formal UAT or regulated release | Dev -> CI -> staging -> approved production deploy | Supports sign-off and integration validation | Staging must have entry/exit criteria and avoid drift |
| Cross-system workflow | External orchestrator invokes dbt deploy step | Coordinates ingestion, dbt, exports, BI refresh, and approvals | Preserve dbt artifacts inside wider run evidence |
| High-risk finance output | Production deploy with service account, reconciliation, retained evidence, and restatement path | Treats deployment as controlled publication | Green dbt status is not enough |
| Frequent production incidents after merge | Slow deployment path and add gates, observability, and rollback/replay runbooks | Reduces blast radius while root causes are addressed | Do not turn process into ceremony without better evidence |
| Multiple teams share one dbt project | Release ownership and branch protection with documented selectors | Prevents conflicting deploys and unclear responsibility | Avoid long-lived divergent branches unless justified |
| Emergency fix | Hotfix branch, focused CI, approved deploy, retained incident evidence | Repairs production quickly while preserving control | Backfill/replay may still be needed after code rollback |

## Questions To Ask

- What does "production" mean: trusted marts, semantic layer, BI extracts, regulatory outputs, or all of these?
- Should production update immediately after merge or on a business schedule?
- Is staging actually validating something CI cannot?
- Which service account, role, warehouse, and target run production deploys?
- Which command and selector define the production deploy?
- Which artifacts are retained and where?
- What proves the deployed commit was approved?
- What freshness, tests, row counts, reconciliation, or publication checks must pass?
- Who owns a failed deploy at night or during close/reporting periods?
- How is bad code rolled back and bad data replayed or restated?

## Related Learning Topics

- [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests]]
- [[02 dbt/05 Deployment CI CD and Operations/41 Dev CI Staging and Prod Environments]]
- [[02 dbt/05 Deployment CI CD and Operations/42 CI Jobs and Slim CI]]
- [[02 dbt/05 Deployment CI CD and Operations/43 Deploy Jobs and Merge Jobs]]
- [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results]]
- [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC]]
- [[02 dbt/05 Deployment CI CD and Operations/48 Incident Response Rollback and Replay]]
- [[02 dbt/05 Deployment CI CD and Operations/49 Observability with dbt and Snowflake Metadata]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - CI Jobs vs Deploy Jobs]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Full CI vs Slim CI]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Environment and Credential Strategy]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Scheduling and Orchestration Pattern]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Cross-Tool/Scenario - Manual Snowflake Changes Keep Breaking Dev Test Prod Consistency]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Analyst Accidentally Ran dbt Against Production]]

## Sources To Revisit

- [dbt Developer Hub - Continuous integration jobs](https://docs.getdbt.com/docs/deploy/ci-jobs)
- [dbt Developer Hub - Job scheduler](https://docs.getdbt.com/docs/deploy/job-scheduler)
- [dbt Developer Hub - dbt environments](https://docs.getdbt.com/docs/dbt-platform-environments)
- [dbt Developer Hub - Run results JSON file](https://docs.getdbt.com/reference/artifacts/run-results-json)

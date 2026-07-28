---
tags:
  - note-comparison
---

# Comparison - CI Jobs vs Deploy Jobs

> CI jobs decide whether proposed code is safe to merge; deploy jobs run approved code to create or update trusted data.

## Short Answer

Use a **CI job** before merge to validate a pull request in an isolated, temporary target with no production write privileges.

Use a **deploy job** after approval or merge to run the accepted code under a controlled production identity and produce consumer-facing data.

## Comparison Table

| Dimension | CI job | Deploy job |
|---|---|---|
| Primary purpose | Validate proposed code | Build or update trusted data from approved code |
| Trigger | Pull request opened, updated, or manually checked | Merge, release approval, schedule, event, or orchestration dependency |
| Code status | Candidate commit | Approved commit or release |
| Write target | Temporary CI schema/database | Persistent production or staging schemas |
| Identity | CI service account | Deployment or production service account |
| Production write access | Should not have it | Has narrowly scoped production write access |
| Typical command | `dbt build --select state:modified+ --target ci` or full CI | `dbt build --target prod` or approved production selector |
| Evidence produced | PR check result, artifacts, logs, selected scope | Production artifacts, logs, query history, freshness, reconciliation |
| Failure meaning | Change is not merge-ready or validation context failed | Trusted data may be late, partial, or not updated |
| Operational owner | Development platform or analytics engineering workflow | Production data operations owner |
| Main risk | False confidence from too-narrow checks | Bad deploy can affect downstream consumers |
| Consultant recommendation | Make it mandatory for protected branches | Treat it as a production operation with identity, artifacts, monitoring, and rollback |

## Decision Rules

- CI should validate the exact candidate commit, not a developer's local state.
- CI should never write production objects.
- A passing CI job makes code eligible to merge; it does not publish production data.
- A deploy job should run only approved code and retain production evidence.
- A failed CI job usually blocks the PR; a failed deploy job is an operational incident.
- Keep service identities separate so a CI credential cannot accidentally become a production deploy credential.
- Retain artifacts for both job types, but treat production deployment evidence as more audit-sensitive.
- Use merge jobs or manifest-refresh jobs deliberately when Slim CI needs a trusted production state artifact.

## Related Learning Topics

- [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests]]
- [[02 dbt/05 Deployment CI CD and Operations/42 CI Jobs and Slim CI]]
- [[02 dbt/05 Deployment CI CD and Operations/43 Deploy Jobs and Merge Jobs]]
- [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC]]
- [[02 dbt/05 Deployment CI CD and Operations/48 Incident Response Rollback and Replay]]

## Related Decisions and Scenarios

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Production Deployment Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Environment and Credential Strategy]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Analyst Accidentally Ran dbt Against Production]]

## Sources To Revisit

- [dbt Developer Hub - Continuous integration jobs](https://docs.getdbt.com/docs/deploy/ci-jobs)
- [dbt Developer Hub - Job scheduler](https://docs.getdbt.com/docs/deploy/job-scheduler)
- [dbt Developer Hub - Deploy jobs](https://docs.getdbt.com/docs/deploy/deploy-jobs)
- [dbt Developer Hub - Run results JSON file](https://docs.getdbt.com/reference/artifacts/run-results-json)

---
status: hub
platform: dbt
area: Deployment CI CD and Operations
tags:
  - dbt
  - dbt-deployment-ops
  - map
---

# Deployment CI CD and Operations Overview

> How dbt becomes an operated production service: Git, environments, CI/CD, orchestration, artifacts, secrets, and incident handling.

## Topics

- [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests|40 - Git Workflow and Pull Requests]]
- [[02 dbt/05 Deployment CI CD and Operations/41 Dev CI Staging and Prod Environments|41 - Dev, CI, Staging, and Prod Environments]]
- [[02 dbt/05 Deployment CI CD and Operations/42 CI Jobs and Slim CI|42 - CI Jobs and Slim CI]]
- [[02 dbt/05 Deployment CI CD and Operations/43 Deploy Jobs and Merge Jobs|43 - Deploy Jobs and Merge Jobs]]
- [[02 dbt/05 Deployment CI CD and Operations/44 Scheduling and Orchestration|44 - Scheduling and Orchestration]]
- [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results|45 - Artifacts, Logs, and Run Results]]
- [[02 dbt/05 Deployment CI CD and Operations/46 Package Management and Dependency Governance|46 - Package Management and Dependency Governance]]
- [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC|47 - Secrets, Service Accounts, and RBAC]]
- [[02 dbt/05 Deployment CI CD and Operations/48 Incident Response Rollback and Replay|48 - Incident Response, Rollback, and Replay]]
- [[02 dbt/05 Deployment CI CD and Operations/49 Observability with dbt and Snowflake Metadata|49 - Observability with dbt and Snowflake Metadata]]

## Topic Summaries

### [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests|40 - Git Workflow and Pull Requests]]

dbt projects are codebases; review discipline matters.

### [[02 dbt/05 Deployment CI CD and Operations/41 Dev CI Staging and Prod Environments|41 - Dev, CI, Staging, and Prod Environments]]

Prevents local testing, CI validation, and production execution from stepping on each other.

### [[02 dbt/05 Deployment CI CD and Operations/42 CI Jobs and Slim CI|42 - CI Jobs and Slim CI]]

Builds only modified resources and their dependents where possible.

### [[02 dbt/05 Deployment CI CD and Operations/43 Deploy Jobs and Merge Jobs|43 - Deploy Jobs and Merge Jobs]]

Defines how approved code becomes production data.

### [[02 dbt/05 Deployment CI CD and Operations/44 Scheduling and Orchestration|44 - Scheduling and Orchestration]]

Compares dbt jobs, Snowflake Tasks, Airflow, Dagster, and enterprise schedulers.

### [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results|45 - Artifacts, Logs, and Run Results]]

Gives operational evidence for debugging, lineage, audit, and incident review.

### [[02 dbt/05 Deployment CI CD and Operations/46 Package Management and Dependency Governance|46 - Package Management and Dependency Governance]]

Covers version pinning, packages.yml, dependencies.yml, private packages, and supply-chain review.

### [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC|47 - Secrets, Service Accounts, and RBAC]]

Central in banking environments where execution identity must be controlled.

### [[02 dbt/05 Deployment CI CD and Operations/48 Incident Response Rollback and Replay|48 - Incident Response, Rollback, and Replay]]

Turns dbt from "SQL that runs" into an operated production service.

### [[02 dbt/05 Deployment CI CD and Operations/49 Observability with dbt and Snowflake Metadata|49 - Observability with dbt and Snowflake Metadata]]

Combines dbt artifacts, job history, Snowflake query history, cost data, and freshness signals.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/04 Incremental Processing and Performance/Incremental Processing and Performance Overview|Previous: Incremental Processing and Performance]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/Governance Semantic Layer and Mesh Overview|Next: Governance Semantic Layer and Mesh]]

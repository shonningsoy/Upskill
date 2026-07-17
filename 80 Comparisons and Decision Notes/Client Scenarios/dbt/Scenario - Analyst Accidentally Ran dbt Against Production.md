---
tags:
  - note-scenario
---

# Scenario - Analyst Accidentally Ran dbt Against Production

> Client says: "A developer thought they were testing locally, but their dbt profile pointed at production and replaced a shared table."

## Likely Reasoning Path

1. Treat this as an environment and privilege design failure, not only a user mistake.
2. Inspect `profiles.yml`, dbt platform credentials, targets, schemas, and Snowflake roles.
3. Confirm whether personal developer credentials can write to production schemas.
4. Separate dev, CI, staging, and prod targets with distinct users, roles, schemas, and warehouses.
5. Use personal dev schemas and restricted production service accounts.
6. Add CI and production deployment paths that do not rely on personal credentials.
7. Consider warehouse, database, and schema naming that makes the active target obvious.
8. Add guardrails, reviews, and recovery steps for accidental production writes.

## Consultant Recommendation Shape

Do not solve this with a reminder to be careful. Developers should not be able to overwrite production objects through a local target. Use separated credentials, least-privilege Snowflake roles, personal schemas, and controlled production jobs.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Small team with no production separation | Add dev and prod targets immediately |
| Developers can write production marts | Remove production write grants from personal roles |
| CI writes to shared schemas | Use temporary or PR-specific CI schemas |
| Production runs use personal credentials | Move to service account or platform credential |
| Regulated environment | Require staged promotion, approval, and audit evidence |

## Watch-outs

- `target.name` does not protect production if the underlying role has broad privileges.
- Personal schemas reduce collisions but do not replace Snowflake RBAC.
- Environment-specific logic can hide bugs if dev and prod behave differently.
- Production recovery may require Time Travel, rebuild, or downstream communication.
- Secrets should not be stored in the project repository.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/04 Environments Profiles Targets and Credentials]]
- [[02 dbt/05 Deployment CI CD and Operations/41 Dev CI Staging and Prod Environments]]
- [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/75 Snowflake RBAC for dbt]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Environment and Credential Strategy]]
- [[80 Comparisons and Decision Notes/Decision Notes/Cross-Tool/Decisions - Choosing a Snowflake DevOps and Deployment Pattern]]

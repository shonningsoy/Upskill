---
tags:
  - note-scenario
---

# Scenario - Manual Snowflake Changes Keep Breaking Dev Test Prod Consistency

> Client says: "Our dev, test, and prod Snowflake accounts do not match. Admins fix things manually, deployments are inconsistent, and nobody knows which changes are safe to repeat."

## Likely Reasoning Path

1. Inventory what differs across environments: warehouses, databases, schemas, roles, grants, integrations, tasks, pipes, app code, dbt models, notebooks, and parameters.
2. Split the estate into categories:
   - stable platform objects,
   - application/project artifacts,
   - transformation models,
   - exploratory notebooks,
   - packaged products.
3. Use Terraform for stable account/platform objects such as warehouses, roles, grants, databases, schemas, and integrations where supported.
4. Use Snowflake CLI for deployable Snowflake project artifacts and SQL/app deployment steps.
5. Use dbt for transformation DAGs, tests, documentation, and model promotion.
6. Use Git Integration when Snowflake itself should fetch source-controlled files.
7. Put all production changes through pull requests and CI/CD, with a documented emergency-change path.
8. Import or codify existing production objects before applying Terraform broadly.
9. Separate developer identities from service users and prefer workload identity/OIDC where supported.
10. Add drift checks, release notes, audit evidence, and ownership for every deployment path.

## Consultant Recommendation Shape

Do not pick one tool for everything. Recommend a layered Snowflake DevOps model:

- **Terraform** for stable infrastructure and RBAC baseline.
- **Snowflake CLI** for project deployment and SQL/app execution.
- **dbt** for transformation models and tests.
- **Git Integration** for Snowflake-readable source files.
- **Notebooks** for exploration, with a promotion path if they become production.
- **Native Apps** only when the output is an installable cross-account product.

The key governance move is to stop manual production changes from being the normal path. Manual changes should either be emergency-only and backported to code, or blocked by process.

## What Good Looks Like

- Dev/test/prod differences are intentional and parameterized.
- Production changes have a pull request, reviewer, run log, and rollback plan.
- Terraform plans are reviewed before apply.
- CI/CD service users are least-privileged.
- Secrets and Terraform state are protected.
- Query tags, deployment logs, and Snowflake history provide audit evidence.
- Manual hotfixes are documented and backfilled into code.

## When to Avoid Overengineering

- A small single-account team may not need full multi-environment automation on day one.
- One-off experiments should not be forced into Terraform.
- New Snowflake features may temporarily need SQL/CLI deployment before provider support matures.
- Not every notebook or script deserves production ceremony.

## Related Learning Topics

- [[01 Snowflake/07 Ecosystem and Integration/48 Snowflake CLI and Terraform Provider]]
- [[01 Snowflake/07 Ecosystem and Integration/49 Git Integration]]
- [[01 Snowflake/07 Ecosystem and Integration/51 Native Apps Framework]]
- [[01 Snowflake/04 Data Engineering/25 dbt on Snowflake]]
- [[01 Snowflake/05 Advanced Analytics and AI/36 Snowflake Notebooks]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Cross-Tool/Decisions - Choosing a Snowflake DevOps and Deployment Pattern]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Snowflake CLI vs Terraform Provider]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - dbt Projects on Snowflake vs dbt Platform]]

## Questions To Ask

- Which Snowflake objects differ across environments today?
- Which changes are infrastructure, transformations, project artifacts, or exploratory work?
- Which objects already exist in prod and need to be imported or codified?
- Who can still make manual production changes?
- Which CI/CD identity runs deployments, and what can it do?
- Where are secrets, OIDC trust, and Terraform state managed?
- How are emergency changes reviewed and backported?

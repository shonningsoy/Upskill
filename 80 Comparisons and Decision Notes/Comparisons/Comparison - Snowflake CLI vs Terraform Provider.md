---
tags:
  - note-comparison
---

# Comparison - Snowflake CLI vs Terraform Provider

> Snowflake CLI is best for executing and deploying Snowflake work; the Terraform Provider is best for declaring stable Snowflake infrastructure as code.

## Short Answer

Use **Snowflake CLI** when the client needs a repeatable way to run SQL or deploy Snowflake projects such as Snowpark, Streamlit, notebooks, Native Apps, SPCS, or Git-connected work. Use **Terraform Provider** when the client needs account infrastructure, roles, grants, warehouses, databases, schemas, and other supported objects managed as reviewed desired state.

In mature Snowflake DevOps, the answer is often **both**: Terraform builds the governed platform baseline, while Snowflake CLI deploys application/project artifacts.

## Comparison Table

| Dimension | Snowflake CLI | Terraform Provider |
|---|---|---|
| Mental model | Command-line deployment and execution | Desired-state infrastructure-as-code |
| Main verb | Run/deploy/manage | Plan/apply/reconcile |
| Best fit | SQL scripts, Snowpark, Streamlit, notebooks, Native Apps, SPCS, stages, object commands | Warehouses, databases, schemas, roles, grants, account objects, stable platform baseline |
| State tracking | No long-term desired-state tracking by default | Uses Terraform state |
| Review artifact | Commands, SQL/YAML/app diffs, CI logs | HCL diffs and Terraform plans |
| Drift handling | Manual detection unless scripted | Built into Terraform workflow |
| Risk surface | Secrets in config/project folders, over-privileged service user, imperative scripts | Sensitive state, accidental destructive plans, provider support gaps |
| Consultant shorthand | Deployment remote control | Desired-state contract |

## Decision Rules

- If the client asks, "How do we deploy this Snowflake project from CI?", start with Snowflake CLI.
- If the client asks, "How do we make every environment have the same warehouses, schemas, roles, and grants?", start with Terraform.
- If the object changes frequently as part of application development, consider CLI, dbt, SQL migrations, or app-specific deployment before Terraform.
- If the object is a stable platform primitive, consider Terraform before manual setup.
- If the team cannot protect Terraform state, fix the state/security model before recommending Terraform broadly.
- If provider support is missing for a new Snowflake feature, use CLI or SQL for now and revisit Terraform later.
- Use Git PRs, CI logs, and Snowflake metadata as the audit trail; do not pretend the tool alone creates governance.

## Related Learning Topics

- [[01 Snowflake/07 Ecosystem and Integration/48 Snowflake CLI and Terraform Provider]]
- [[01 Snowflake/07 Ecosystem and Integration/49 Git Integration]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging]]
- [[01 Snowflake/05 Advanced Analytics and AI/32 Snowpark]]
- [[01 Snowflake/05 Advanced Analytics and AI/36 Snowflake Notebooks]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Manual Snowflake Changes Keep Breaking Dev Test Prod Consistency]]

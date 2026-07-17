---
tags:
  - note-comparison
---

# Comparison - Seeds vs Managed Reference Tables

> Seeds are Git-controlled CSV inputs for small static reference data. Managed reference tables are better for governed, changing, sensitive, or operationally owned data.

## Short Answer

Choose a **dbt seed** when the data is small, stable, non-sensitive, and benefits from pull-request review in the dbt repository.

Choose a **managed reference table** when the data changes often, belongs to business operations, needs approvals outside Git, contains sensitive values, or is too large for a CSV-based workflow.

The decisive question is:

```text
Is this really static project reference data, or is it operational data with ownership and controls?
```

## Comparison Table

| Dimension | dbt seed | Managed reference table |
|---|---|---|
| Storage before load | CSV file in the dbt project | Warehouse table, application table, MDM system, or governed source |
| Loaded by | `dbt seed` | Ingestion, app workflow, SQL process, or data-management process |
| Best for | Small mappings, allowed values, test fixtures, simple static lists | Large mappings, changing rates, account ownership, regulatory parameters |
| Change process | Git commit and pull request | Business workflow, data steward approval, application update, or controlled SQL process |
| Versioning | Git history | Data history, audit table, CDC, snapshots, or source-system logs |
| Sensitivity | Avoid sensitive data | Can use warehouse RBAC, masking, audit, and stewardship controls |
| Scale | Small | Small to large |
| Ownership | Analytics engineering or dbt project owner | Business owner, data steward, platform owner, or application team |

## Decision Rules

- Use seeds for small stable lookup tables that are genuinely part of the project logic.
- Use managed tables when the data is maintained by business users or operations.
- Use managed tables when changes need maker-checker approval or audit evidence outside Git.
- Use managed tables when the data contains personal, regulated, commercial, or security-sensitive values.
- Avoid seeds for frequently changing exchange rates, customer mappings, user-maintained override lists, or production control tables.
- Consider a seed for test data or a tiny list of allowed statuses, regions, or category mappings.

## Consultant Recommendation Shape

> "A seed is a simple way to put tiny static reference data under code review. Once the data becomes business-owned, sensitive, frequently updated, or audit-critical, we should move it into a governed table and let dbt reference it like any other source."

## Watch-outs

- Seeds can quietly become production control tables without proper ownership.
- Large seed files slow down development, code review, and deployment.
- Sensitive CSVs in Git are hard to remove from history after exposure.
- Business users rarely want to open pull requests to update operational reference data.
- A seed gives Git history, but not necessarily business-effective dating or approval evidence.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/08 Seeds and Static Reference Data]]
- [[02 dbt/01 Core Concepts and Project Structure/05 Models ref source and the DAG]]
- [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness]]
- [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests]]
- [[02 dbt/03 Testing Documentation and Data Quality/29 Data Quality Strategy in Regulated Environments]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Static Mapping CSV Became a Production Control Risk]]

## Sources To Revisit

- [dbt Docs: Seeds](https://docs.getdbt.com/docs/build/seeds)
- [dbt Docs: Seed configurations](https://docs.getdbt.com/reference/seed-configs)
- [dbt Docs: Source properties](https://docs.getdbt.com/reference/source-properties)

---
tags:
  - note-scenario
---

# Scenario - Static Mapping CSV Became a Production Control Risk

> Client says: "We started with a small CSV seed for product mappings, but now business users ask for weekly changes and the file affects financial reporting."

## Likely Reasoning Path

1. Confirm what the seed controls and which reports depend on it.
2. Check whether the data is still small, stable, non-sensitive, and analytics-owned.
3. Identify who should approve changes and whether Git pull requests are acceptable for that workflow.
4. If the mapping is business-owned or audit-critical, move it to a governed reference table.
5. Keep dbt models using `ref()` or `source()` cleanly so downstream logic does not care where the mapping is maintained.
6. Add tests for accepted values, duplicate keys, missing mappings, and effective-date overlaps if needed.
7. Preserve history if mappings need to be reconstructed for prior reports.

## Consultant Recommendation Shape

The seed was a good starting point, but the workflow has outgrown it. Once a CSV becomes a production control table, the recommendation should shift toward governed reference data with ownership, approvals, access control, and history.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Tiny static list maintained by analytics engineers | Keep as dbt seed |
| Weekly changes by business users | Managed reference table |
| Sensitive or regulated mapping values | Managed table with RBAC and audit controls |
| Report needs historical mapping logic | Effective-dated reference table or snapshot |
| Seed still used for tests or examples | Keep separate test fixture seed |

## Watch-outs

- Git history is not the same as business approval history.
- Sensitive values in CSVs can remain in repository history.
- Business users may bypass analytics if the update process is too awkward.
- Moving from seed to source should include downstream regression checks.
- Historical restatement rules must be explicit before changing mappings used in finance.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/08 Seeds and Static Reference Data]]
- [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking]]
- [[02 dbt/03 Testing Documentation and Data Quality/21 Generic Singular and Custom Data Tests]]
- [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests]]
- [[02 dbt/03 Testing Documentation and Data Quality/29 Data Quality Strategy in Regulated Environments]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Seeds vs Managed Reference Tables]]

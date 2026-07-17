---
tags:
  - note-scenario
---

# Scenario - Team Cannot Tell Which Dashboards a Model Change Will Break

> Client says: "Before changing a mart model, we have to ask around manually because nobody knows which dashboards, reports, or ML jobs depend on it."

## Likely Reasoning Path

1. Confirm that dbt models use `ref()` and `source()` rather than hard-coded object names.
2. Generate docs and inspect lineage from sources through staging, marts, and downstream resources.
3. Add exposures for important dashboards, reports, notebooks, ML jobs, and reverse-ETL outputs.
4. Add owners and maturity levels to exposures so impact review has a named contact.
5. Use node selection to build or test affected upstream/downstream resources.
6. Add documentation for important models and columns, especially published marts.
7. Make exposure updates part of the release checklist when new downstream assets go live.

## Consultant Recommendation Shape

This is a metadata and operating-model problem. dbt can show model lineage, but downstream impact analysis needs exposures and ownership. Add exposures for important business-facing assets so model changes can be reviewed against real consumers.

## What To Recommend

| Situation | Recommendation |
|---|---|
| dbt lineage stops at marts | Add exposures for dashboards and reports |
| Models use hard-coded table names | Replace with `ref()` and `source()` where appropriate |
| No owner for downstream dashboards | Add exposure owners and maturity |
| High-risk model change | Use lineage to identify and test affected resources |
| Many low-value ad hoc dashboards | Document critical exposures first |

## Watch-outs

- Exposures must be maintained; stale exposure metadata creates false confidence.
- BI tools may have their own lineage, but dbt needs explicit exposure definitions to understand downstream consumers.
- Docs without ownership still leave teams unsure who to contact.
- Lineage shows dependency, not business approval.
- Critical dashboards should not rely on tribal knowledge as the impact-analysis process.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/05 Models ref source and the DAG]]
- [[02 dbt/01 Core Concepts and Project Structure/10 Documentation Lineage and Exposures]]
- [[02 dbt/01 Core Concepts and Project Structure/06 Commands and Artifacts]]
- [[02 dbt/03 Testing Documentation and Data Quality/24 Documentation Blocks and Catalog]]
- [[02 dbt/03 Testing Documentation and Data Quality/25 Exposures]]

## Related Decision Notes

- No related decision note yet.

---
tags:
  - note-comparison
---

# Comparison - Conformed Analytical Models vs Master Data Management

> Analytical conformance makes reporting consistent; master data management governs authoritative identities and operational master data across systems.

## Short Answer

Use a **conformed analytical model in dbt** when known source records and business mappings must be standardized into reusable dimensions, entities, or marts for analysis.

Use **master data management (MDM)** when the organization needs authoritative entity resolution, stewardship workflows, managed match decisions, or master data that operational systems also consume.

Use both when MDM should govern identity and master attributes while dbt shapes that mastered data into analytical models.

## Comparison Table

| Dimension | Conformed analytical model | Master data management |
|---|---|---|
| Primary purpose | Consistent analytics across sources and facts | Governed enterprise master data and identity |
| Typical output | Conformed dimension, entity model, crosswalk, or mart | Master entity, golden record, relationships, and mastered attributes |
| Main users | Analysts, finance, risk, data products, semantic models | Operational applications, data stewards, governance teams, and analytics |
| Identity approach | Deterministic mappings and approved analytical rules | Deterministic, probabilistic, or ML-assisted matching with stewardship |
| Ambiguous matches | Usually exposed as unmatched or conflicted records | Can route potential matches to human review and remediation workflows |
| Authority claim | Trusted for a declared analytical purpose | Intended to establish or manage authoritative master data |
| Operational writeback | Normally no; dbt builds warehouse relations | May publish mastered identities and attributes back to consuming systems |
| Change process | Code review, tests, documentation, and model ownership | Governance policy, stewardship, workflow, match tuning, and audit trail |
| Latency | Usually batch or scheduled analytical refresh | Often designed for broader operational and near-real-time use |
| Main risk | Presenting uncertain matches as trusted business truth | High implementation, governance, integration, and stewardship overhead |

## Decision Rules

- Use dbt conformance when identity mappings are known, analytics is the main consumer, and exceptions can be handled as data-quality outputs.
- Use MDM when uncertain identity resolution requires match scoring, human stewardship, link/unlink decisions, or operational publication.
- Do not call a source-qualified surrogate key a golden customer key; collision protection is not entity resolution.
- Do not introduce MDM merely because several sources use different column names or status codes; dbt can handle deterministic analytical standardization.
- Do not build an unofficial golden record in dbt when no business owner can approve matching and precedence rules.
- A strong hybrid lets MDM own mastered identity while dbt owns analytical grains, facts, historical joins, measures, and marts.

## Example

```text
CRM contact -------+
Core party --------+--> MDM identity and stewardship --> mastered customer ID
Cardholder --------+                                  |
                                                       v
                                      dbt conformed dimensions and marts
                                                       |
                                                       v
                                          finance, risk, and customer analytics
```

Without MDM, dbt may still build a useful analytical crosswalk when matches are deterministic and governed:

```text
customer_key | source_system | source_customer_id | mapping_method
100023       | CRM           | 1842               | verified national ID
100023       | CORE          | 98411              | verified national ID
```

## Consultant Recommendation Shape

> “Use dbt to make known entities and definitions consistent for analytics. Introduce MDM when the requirement becomes authoritative identity resolution with stewardship and operational consumption. If both are needed, let MDM govern the master identity and let dbt turn it into analytical facts, dimensions, history, and data products.”

## Watch-outs

- Do not merge records solely because source identifiers happen to match.
- Do not hide ambiguous matches inside `coalesce()` or undocumented precedence rules.
- Preserve source identifiers, mapping methods, confidence, and unresolved cases.
- Confirm whether the client means an analytical customer view, a customer 360, or an operational golden record; these are not automatically the same deliverable.
- MDM is not only a technology purchase. It requires ownership, stewardship capacity, integration, and resolution policy.
- A mastered current entity does not remove the need for effective-dated analytical history.

## Related Learning Topics

- [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products]]
- [[02 dbt/02 Modeling Patterns and Layering/14 Dimensional Modeling with dbt]]
- [[02 dbt/02 Modeling Patterns and Layering/18 Multi-source Conformed Models]]

## Related Decisions and Comparisons

- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Star Schema vs Wide Marts]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Modeling Layer]]

## Sources To Revisit

- [Kimball Group: Conformed Dimensions](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/conformed-dimension/)
- [IBM: What is Master Data Management?](https://www.ibm.com/think/topics/master-data-management)
- [IBM Docs: Completing data stewardship tasks in Master Data Management](https://www.ibm.com/docs/en/cloud-paks/cp-data/5.4.x?topic=data-completing-stewardship-tasks)
- [dbt Labs: Marts - Business-defined entities](https://docs.getdbt.com/best-practices/how-we-structure/4-marts)

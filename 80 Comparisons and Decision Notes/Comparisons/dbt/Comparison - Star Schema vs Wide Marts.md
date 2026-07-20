---
tags:
  - note-comparison
---

# Comparison - Star Schema vs Wide Marts

> Star schemas maximize reusable dimensions and explicit relationships; wide marts minimize consumer joins by packaging relevant context with the core entity.

## Short Answer

Choose a **star schema** when multiple facts need shared dimensions, conformed analysis matters, and consumers or a semantic layer can manage explicit joins.

Choose a **wide mart** when the primary goal is simple, fast consumption of one entity and repeatedly joining descriptive context would add cost or error risk.

The decision is not ideological. Start with grain and consumers, then choose the physical shape that makes correct use easiest.

## Comparison Table

| Dimension | Star schema | Wide mart |
|---|---|---|
| Physical shape | Central fact joined to reusable dimensions | Entity-grained table containing facts and useful descriptive attributes |
| Consumer experience | Requires correct joins | Fewer joins and simpler queries |
| Reuse | Dimensions shared across many facts | Context repeated across marts |
| Conformance | Strong place for shared entity definitions | Must keep duplicated attributes synchronized |
| Query compute | Repeated joins consume compute | Prejoined data can reduce repeated compute |
| Storage | Less repeated descriptive data | More duplicated attributes, usually acceptable on modern warehouses |
| History | Type 2 dimension relationships can be explicit | Historical meaning of copied attributes must be stated carefully |
| Security | Sensitive attributes can remain in governed dimensions | Sensitive fields and policies may be copied into more relations |
| BI usability | Strong for tools and teams that understand dimensional joins | Strong for self-service users who need a ready-to-query dataset |
| Semantic Layer fit | Often a natural fit for entity relationships | May hide reusable entities and repeat measures |
| Main risk | Incorrect joins or a design too complex for the use case | Duplication, inconsistent attributes, and very wide hard-to-own models |

## Decision Rules

- Start by declaring the core entity and grain; neither design fixes mixed-grain data.
- Use conformed dimensions when several facts need the same instrument, account, customer, counterparty, or date context.
- Use a wide mart when one dominant consumer path repeatedly needs the same related attributes.
- Prefer more normalized entity-aware models when dbt Semantic Layer will construct relationships and metrics.
- Denormalize selectively when measured query cost, usability, or join-error risk justifies it.
- Do not build separate wide versions of the same entity for every dashboard or department.
- Do not expose sensitive dimension attributes in wider marts without repeating the required Snowflake policies and access review.
- A hybrid is common: maintain conformed dimensions and atomic facts, then publish selected wide marts for high-value consumption paths.

## Example

Star schema:

```text
fct_trades
  ├── account_key       → dim_accounts
  ├── instrument_key    → dim_instruments
  ├── counterparty_key  → dim_counterparties
  └── trade_date_key    → dim_dates
```

Wide mart:

```text
trades
  ├── trade_id
  ├── account_name
  ├── instrument_name
  ├── asset_class
  ├── counterparty_country
  ├── trade_date
  ├── quantity
  └── notional_amount
```

Hybrid recommendation:

```text
Conformed dimensions + atomic facts
                  ↓
Selected wide marts for BI, risk, or operational use
```

## Consultant Recommendation Shape

> "Use dimensional facts and conformed dimensions as the reusable analytical foundation when several processes share business context. Add wide marts for consumer paths where fewer joins materially improve usability or performance. If the dbt Semantic Layer will manage joins, preserve normalized entity relationships rather than denormalizing everything upfront."

## Watch-outs

- Do not use a wide mart to hide an undefined or mixed grain.
- Do not assume Snowflake storage being inexpensive removes governance costs from duplicated data.
- Do not force casual users through a complex star if they repeatedly produce incorrect joins.
- Do not use current dimension attributes in historical wide facts without declaring restatement behavior.
- Do not confuse a normalized physical design with governed metric semantics; measures still need aggregation rules.
- Do not optimize by convention. Measure query cost and observe real consumer behavior.

## Related Learning Topics

- [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products]]
- [[02 dbt/02 Modeling Patterns and Layering/14 Dimensional Modeling with dbt]]
- [[02 dbt/02 Modeling Patterns and Layering/18 Multi-source Conformed Models]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/55 Semantic Models and Metrics]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/77 Cost Governance for dbt on Snowflake]]

## Related Decisions and Scenarios

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Modeling Layer]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Business Users Want Self-Service Analytics but Metrics Are Inconsistent]]

## Sources To Revisit

- [Kimball Group: Dimensional Modeling Techniques](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/)
- [dbt Docs: Marts - Business-defined entities](https://docs.getdbt.com/best-practices/how-we-structure/4-marts)
- [dbt Docs: Semantic models](https://docs.getdbt.com/docs/build/semantic-models)

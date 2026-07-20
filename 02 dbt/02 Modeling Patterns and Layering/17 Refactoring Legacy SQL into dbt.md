---
status: active
platform: dbt
area: Modeling Patterns and Layering
topic_number: 17
tags:
  - dbt
  - dbt-modeling
  - learning
---

# Refactoring Legacy SQL into dbt

> Preserve trusted behavior first, then improve structure with evidence and a controlled cutover.

## Executive Summary

- Refactoring is not simply copying old SQL into a dbt model. It is understanding the existing promise, proving parity, and then improving the design safely.
- Triage first: retire unused logic, translate suitable SQL, redesign fragile pipelines, and keep genuinely procedural work outside dbt when needed.
- For business-critical logic, establish an equivalent baseline before changing business rules or architecture.
- Replace hardcoded object names with `source()` and `ref()`, then split logic only at meaningful boundaries.
- Use reconciliation, parallel runs, explicit cutover criteria, and a rollback plan—especially for finance data.

## What It Can Do

- Turn hidden dependencies into a visible dbt DAG.
- Separate source cleanup, reusable transformations, and business-facing outputs.
- Add tests, documentation, ownership, and version control around legacy logic.
- Make changes reviewable and deployments repeatable.
- Retire duplicated or unused transformations after validation.

## What It Cannot Do

- Prove that poorly understood legacy behavior is correct.
- Automatically translate every stored procedure into a good dbt design.
- Preserve multi-table transactions, branching side effects, or notifications by itself.
- Remove the need for business owners to approve material differences.
- Make a risky big-bang cutover safe without operational planning.

## Core Concepts

| Concept | Practical meaning |
|---|---|
| Legacy contract | Grain, schema, population, calculations, timing, and consumers that must be understood. |
| Triage | Decide whether to retire, translate, redesign, or retain each legacy process. |
| Parity baseline | A dbt result that intentionally matches the current output before improvements. |
| Meaningful boundary | Split a model when the logic has its own grain, name, reuse, test, owner, or performance need. |
| Declarative boundary | dbt fits logic that describes the dataset to build from known inputs. |
| Migration audit | Compare old and new outputs by schema, keys, aggregates, and rows. |
| Parallel run | Operate both pipelines long enough to cover normal and exceptional business cycles. |
| Controlled cutover | Switch consumers with owners, acceptance criteria, monitoring, rollback, and a retirement date. |

## How It Works (Simple Flow)

1. Inventory inputs, outputs, schedules, consumers, dependencies, owners, and side effects.
2. Triage each process: retire, translate, redesign, or keep procedural.
3. Record the legacy contract and important edge cases.
4. Build a parity model and replace hardcoded relations with `source()` and `ref()`.
5. Refactor into staging, intermediate, and mart layers only where boundaries add value.
6. Compare legacy and dbt outputs; classify and resolve every material difference.
7. Run both pipelines through representative cycles, including late corrections or month-end.
8. Cut over with monitoring and rollback, then retire the legacy path.

## Visuals

```mermaid
flowchart LR
    A["Discover and triage"] --> B["Capture legacy contract"]
    B --> C["Build parity model"]
    C --> D["Refactor into useful layers"]
    D --> E["Reconcile old vs new"]
    E --> F["Parallel run"]
    F --> G["Cut over and retire"]
```

## Readable Snippets

### Replace hardcoded relations

```sql
-- Legacy
from raw.erp.orders

-- dbt source
from {{ source('erp', 'orders') }}

-- dbt model dependency
join {{ ref('stg_erp__customers') }} using (customer_id)
```

### Refactor around business meaning

```text
stg_erp__orders
    -> int_order_items_aggregated_to_order
        -> fct_orders
```

Do not create one dbt model for every legacy temporary table. Promote a step only when it deserves a stable name, grain, test, reuse point, or operational boundary.

## Consultant Talking Points

- Start with high-value, lower-complexity pipelines to establish a repeatable migration method.
- Ask what the process promises consumers—not only what its SQL currently does.
- Separate architectural refactoring from business-rule changes so differences are explainable.
- Row-count equality is weak evidence; validate keys, totals, important slices, and edge cases.
- A hybrid design is valid when dbt owns declarative transformations and an orchestrator or procedure owns transactional side effects.
- Define who approves differences and who owns rollback before cutover.

## Common Pitfalls

- Migrating unused SQL instead of retiring it.
- Treating one stored procedure as one dbt model.
- Changing architecture and business meaning simultaneously.
- Converting every temporary table into a permanent model.
- Introducing macros or incremental logic before parity is understood.
- Checking only row counts and missing rounding, null, timezone, or duplicate differences.
- Losing transactional or side-effect behavior during translation.
- Running old and new pipelines indefinitely without a retirement decision.

## When to Recommend What (Decision Table)

| Situation | Recommendation |
|---|---|
| Unused or duplicated pipeline | Retire it after confirming no consumers remain. |
| Clear SELECT-based transformation | Translate to dbt and establish parity first. |
| Valuable but tangled SQL | Build a parity model, then refactor in small reviewed steps. |
| Reused temporary result with clear grain | Promote it to an intermediate model. |
| One-use mechanical temporary step | Keep it as a CTE. |
| Insert or merge into one analytical table | Consider an incremental model after correctness is proven. |
| Ordered branching, multi-table transaction, or side effects | Keep a procedure/orchestrator or use a hybrid design. |
| Finance-critical output | Require reconciliation, approvals, representative parallel runs, and rollback. |

## Related Topics

- [[02 dbt/02 Modeling Patterns and Layering/Modeling Patterns and Layering Overview|Modeling Patterns and Layering Overview]]
- [[02 dbt/02 Modeling Patterns and Layering/11 Staging Models|Staging Models]]
- [[02 dbt/02 Modeling Patterns and Layering/12 Intermediate Models|Intermediate Models]]
- [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products|Marts and Data Products]]
- [[02 dbt/02 Modeling Patterns and Layering/16 Naming Conventions and Folder Design|Naming Conventions and Folder Design]]
- [[02 dbt/03 Testing Documentation and Data Quality/28 Audit and Migration Validation|Audit and Migration Validation]]
- [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys|Incremental Models and Unique Keys]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Modeling Layer|Decisions - Choosing the Right dbt Modeling Layer]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Stored Procedures vs Declarative Transformations|Comparison - Stored Procedures vs Declarative Transformations]]

## Questions

1. What is the grain and business contract of the current output?
2. Which legacy behaviors are intentional, accidental, or unknown?
3. Can any pipelines be retired instead of migrated?
4. Which steps need their own model, and which should remain CTEs?
5. What comparisons would prove parity beyond row counts?
6. Does the process rely on transactions, branching, or side effects?
7. Which business cycles must a parallel run cover?
8. Who approves cutover, monitors it, and can trigger rollback?

## Sources To Revisit

- [dbt Labs — Refactoring legacy SQL to dbt](https://www.getdbt.com/blog/sql-refactoring-course)
- [dbt Labs — Start fresh, don't lift and shift](https://www.getdbt.com/blog/start-fresh-don-t-lift-and-shift-a-dbt-migration-guide)
- [dbt Labs — From stored procedures to dbt](https://www.getdbt.com/blog/stored-procedures-dbt-migration-playbook)
- [dbt Labs — audit_helper](https://github.com/dbt-labs/dbt-audit-helper)

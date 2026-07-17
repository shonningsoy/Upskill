---
tags:
  - note-comparison
---

# Comparison - Stored Procedures vs Declarative Transformations

## Short Answer

Use a **stored procedure** when the requirement is “perform this ordered or conditional sequence of actions,” especially when several DML statements, explicit transactions, quarantine, audit updates, dynamic administration, or controlled privilege delegation are involved.

Use **plain SQL, a UDF, a Dynamic Table, or dbt** when the requirement is primarily “calculate or maintain this result.” Declarative solutions are usually easier to understand, test, optimize, and trace.

The decisive question is:

```text
Is the requirement a result, or an operation with side effects?
```

## Comparison Table

| Dimension | Plain SQL / UDF | Dynamic Table | dbt model/DAG | Stored procedure |
|---|---|---|---|---|
| Primary intent | Calculate a set or reusable value | Maintain a query result toward a target lag | Build tested, documented analytical models | Perform a reusable sequence of actions |
| Style | Declarative | Declarative | Declarative models plus build orchestration | Imperative/procedural |
| Typical invocation | Query or calling SQL | Snowflake refresh | dbt job, Task, or orchestrator | `CALL`, often from a Task |
| Side effects | Avoid; UDFs are value-oriented | Produces/refreshes its own result | Primarily materializes models and tests them | Central use case: DML, audit, quarantine, administration |
| Branching | SQL expressions | SQL expressions | Jinja/build selection, not runtime business side effects | `IF`, `CASE`, exceptions, loops |
| Multiple DML statements | Poor fit | No | Possible through specialized patterns, but not the normal model contract | Natural fit |
| Explicit transaction control | One statement is already atomic | Managed refresh | Not normally the model author’s unit of control | Available and often essential |
| Privilege delegation | Normal object privileges | Normal object privileges | Execution role and platform controls | Owner’s/restricted caller’s rights can expose a narrow controlled capability |
| Testing and documentation | Manual unless wrapped in a framework | Snowflake monitoring and external tests | Strong built-in project workflow | Must be designed through code, test harnesses, audit, and deployment process |
| Lineage | Query/object lineage | Snowflake dependency graph | dbt DAG plus platform/Snowflake lineage | Harder when dynamic SQL and side effects are extensive |
| Performance default | Set-based and optimizer-friendly | Set-based managed refresh | Set-based model SQL | Good when statements are set-based; poor when loops process rows |
| Best example | Calculate signed quantity | Maintain current exposure | Build position and risk marts | Validate, quarantine, merge, audit, and publish a batch |

## Include Tasks and Streams Correctly

A Task is not an alternative implementation of the business logic. It is an execution primitive:

```text
Stream    = what changed
Task      = when to act
Procedure = how to coordinate several actions
```

If one scheduled SQL statement is enough, let the Task run it directly. Add a procedure only when it creates a meaningful reusable transaction, security boundary, or multi-step operation.

## Decision Rules

- **One set-based operation:** use plain SQL.
- **One reusable calculation inside queries:** use a SQL expression or UDF.
- **One SQL statement on a cadence:** use a Task directly.
- **Maintain a declarative query result:** use a Dynamic Table.
- **Build a governed analytical model graph:** use dbt.
- **Validate, branch, quarantine, merge, audit, or administer in a coordinated operation:** use a stored procedure.
- **Detect changes and invoke that operation:** add a Stream and Task.
- **Coordinate databases, APIs, files, approvals, and complex backfills:** use an external orchestrator.

## Transaction and Failure Rule

Stored procedures provide control, not automatic safety:

```text
Explicit transaction + rollback = protect the data
Re-raised exception              = tell the scheduler the truth
Idempotency                       = make retry safe
```

If the procedure catches an unexpected error and returns `'FAILED'`, the Task may record success. If it rolls back but is not idempotent, an operator rerun can still repeat external or previously committed side effects.

## Security Rule

Choose owner’s rights only when controlled delegation is intentional. The procedure owner should be a narrowly privileged application role, inputs must be constrained, and dynamic SQL must not turn the procedure into a general-purpose privilege tunnel.

Caller’s rights is the safer default when callers already possess the necessary object access. Restricted caller’s rights fits when caller context is needed but only an approved subset of caller privileges should be usable.

## Consultant Recommendation Shape

> “Keep transformations declarative until the requirement clearly becomes an operation with ordered side effects. Introduce a procedure at that boundary, make the transaction and security model explicit, then let a Task or orchestrator decide when to call it.”

## Related Learning Topics

- [[01 Snowflake/04 Data Engineering/26 Stored Procedures]]
- [[01 Snowflake/04 Data Engineering/20 Streams and Tasks]]
- [[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
- [[01 Snowflake/04 Data Engineering/25 dbt on Snowflake]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Trade Batch Must Be Validated Before Publication]]

## Sources To Revisit

- [Snowflake Docs: Stored Procedures Overview](https://docs.snowflake.com/en/developer-guide/stored-procedure/stored-procedures-overview)
- [Snowflake Docs: Working with Stored Procedures](https://docs.snowflake.com/en/developer-guide/stored-procedure/stored-procedures-usage)
- [Snowflake Docs: Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables-about)
- [dbt Docs: SQL Models](https://docs.getdbt.com/docs/build/sql-models)

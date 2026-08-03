---
tags:
  - note-comparison
---

# Comparison - Hooks vs Models Tests and Operations

## Short Answer

Use a **model** to create a durable transformed dataset, a **test** to return failing records or validate logic, a **hook** for tightly coupled lifecycle SQL, and an **operation** for an explicit administrative action.

Do not use hooks as an invisible orchestration framework. Prefer the dbt resource whose lifecycle and evidence match the job.

## Comparison Table

| Dimension | Model | Test | Hook | Operation |
|---|---|---|---|---|
| Primary purpose | Build a relation or transformation | Evaluate a quality expectation | Run SQL around a resource or invocation lifecycle | Invoke a macro explicitly |
| Trigger | Selected `run` or `build` | Selected `test` or `build` | Configured pre/post or run start/end event | `dbt run-operation` |
| DAG visibility | First-class node | First-class test node | Attached behavior, less visible | Outside normal model DAG |
| Best use | Business transformations and audit tables | Data-quality failures and reusable checks | Narrow grants, session setup, or lifecycle logging | Controlled maintenance or one-off administration |
| Main risk | Wrong grain or materialization | Noisy/expensive test suite | Hidden side effects and partial-failure ambiguity | Manual misuse and weak approval evidence |
| Evidence | Relation, artifacts, run result | Test result and artifacts | Logs and warehouse history | Logs, arguments, and change record |

## Decision Rules

- If the output is data that downstream consumers depend on, make it a model.
- If success is defined as returning zero invalid rows, make it a test.
- If SQL must happen immediately before or after one resource, a small idempotent hook may fit.
- If an action should be deliberately invoked with reviewed arguments, use an operation.
- Use the orchestrator or warehouse-native scheduler for cross-system workflow, approvals, retries, and long-running control flow.
- Prefer dbt grants/configuration over custom grant hooks when supported.
- Keep hooks small, observable, and safe to retry; document transaction behavior for the adapter.
- Treat destructive operations as privileged runbooks, not convenient developer utilities.

## Examples

```yaml
models:
  - name: fct_positions
    config:
      post_hook:
        - "insert into audit.model_runs(model_name) values ('fct_positions')"
```

This may be acceptable for tightly coupled audit logging if it is safe to retry. A reconciliation dataset needed by consumers should instead be a model; a failure rule should be a test.

```bash
dbt run-operation apply_approved_maintenance --args '{change_id: CHG-1042}'
```

An operation makes the invocation explicit, but the organization still needs authorization, logging, idempotency, and rollback rules.

## Related Learning Topics

- [[02 dbt/07 Packages Macros and Advanced Reuse/69 Custom Generic Tests]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/71 Hooks and Operations]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/72 Advanced Macro Boundaries]]
- [[02 dbt/03 Testing Documentation and Data Quality/21 Generic Singular and Custom Data Tests]]
- [[02 dbt/05 Deployment CI CD and Operations/44 Scheduling and Orchestration]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Quality Control]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Scheduling and Orchestration Pattern]]

## Sources To Revisit

- [dbt Developer Hub - Hooks and operations](https://docs.getdbt.com/docs/build/hooks-operations)
- [dbt Developer Hub - run-operation](https://docs.getdbt.com/reference/commands/run-operation)
- [dbt Developer Hub - Data tests](https://docs.getdbt.com/docs/build/data-tests)

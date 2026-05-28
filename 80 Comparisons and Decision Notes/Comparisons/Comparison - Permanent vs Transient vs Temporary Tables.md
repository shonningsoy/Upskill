---
tags:
  - note-comparison
---

# Comparison - Permanent vs Transient vs Temporary Tables

> Choose table type by recovery value, rebuildability, and lifecycle.

## Short Answer

Use **permanent tables** for important durable data. Use **transient tables** for rebuildable data that should not pay for Fail-safe. Use **temporary tables** for session-scoped scratch work.

## Comparison Table

| Dimension | Permanent | Transient | Temporary |
|---|---|---|---|
| Best for | Production facts/dimensions, governed data | Staging, intermediate, rebuildable outputs | Session-only analysis or procedure work |
| Time Travel | Yes, based on retention | Limited | Limited/session-oriented |
| Fail-safe | Yes | No | No |
| Cost posture | More protection, potential storage overhead | Lower protection/cost | Disposable |
| Consultant warning | Do not use everywhere by default | Confirm rebuildability and SLA | Do not rely on it for shared durable data |

## Decision Rules

- If the data is hard to reconstruct or regulated, lean permanent.
- If the data can be rebuilt from source and does not need Fail-safe, consider transient.
- If the data is scratch/session-only, use temporary.
- Long retention on large rebuilt permanent staging tables can create surprise storage cost.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/30 Account Usage Views]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Snowflake Costs Spiked After Retention Change]]

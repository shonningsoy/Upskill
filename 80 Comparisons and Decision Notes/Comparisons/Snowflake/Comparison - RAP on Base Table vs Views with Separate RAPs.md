---
tags:
  - note-comparison
  - sf-security-governance
---

# Comparison - RAP on Base Table vs Views with Separate RAPs

## Short Answer

Use **a single RAP on the base table** when multiple teams need the same columns and filtering logic stays manageable (2-3 dimensions, straightforward OR logic).

Use **separate views with simpler RAPs** when teams need different column projections, the filtering logic would become unreadably complex, or you need different masking policies per audience.

## Comparison Table

| Dimension | Single RAP on base table | Separate views with per-view RAPs |
|---|---|---|
| Primary purpose | One policy governs all row-level access to a shared table | Each team gets a tailored view with its own simpler policy |
| Strengths | Single governance artifact (one mapping table); no view maintenance; one place to audit | Simpler policies per team; can also control column projections; easier to combine with different masking |
| Limits | Policy body grows complex with many teams/dimensions; hard to read and test | Must lock down base table access; views must stay in sync with schema changes; more objects to manage |
| Cost considerations | One policy evaluation per query — but complex policy logic can add latency | Multiple simpler evaluations — but additional view layer doesn't add compute cost |
| Governance considerations | Mapping table is the single source of truth — must be treated as security artifact | Must ensure no one bypasses views to hit base table directly; more objects to audit |
| Consultant recommendation | Default choice for shared tables with straightforward multi-team filtering | Choose when teams fundamentally differ in what they need to see (rows AND columns) |

## Decision Rules

- If all teams need the same columns and just different row subsets → single RAP on base table.
- If teams need different columns AND different rows → views with separate RAPs.
- If the policy body exceeds ~30 lines of logic or has more than 3 OR branches → consider splitting into views.
- If you need different column masking per team on the same table → views are required (one masking policy per table).
- If operational simplicity is paramount (fewer objects, fewer DDL changes) → single RAP.

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]

## Related Scenarios

- 

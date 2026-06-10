---
tags:
  - note-decision
  - sf-security-governance
---

# Decisions - Choosing a Row-Level Data Isolation Strategy

> When a client needs different users to see different rows in shared tables, which mechanism should you reach for — and when is each one overkill or insufficient?

## Decision Frame

A client has tables shared across multiple teams, regions, or tenants. They need row-level separation so each group sees only their permitted data. The question is whether to use RBAC alone (separate objects), Row Access Policies, views with policies, or some combination. This decision affects governance complexity, performance, and operational maintainability.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Small number of teams, each needs a completely separate dataset | RBAC only — separate schemas or tables per team | Simplest; no policy logic; clean ownership boundaries | Doesn't scale if teams grow or data overlaps |
| Shared table, teams need different row subsets, same columns | Single RAP with a mapping table | One governance artifact; no view proliferation; centrally auditable | Mapping table becomes a critical security object; policy complexity grows with dimensions |
| Shared table, teams need different rows AND different columns | Views per team + simpler RAPs on each view | Column projections handled by view; simpler policy per audience | Must lock base table; more objects to maintain; views must track schema changes |
| Very simple "see only your own data" requirement | RAP using `CURRENT_USER()` directly | No mapping table needed; minimal overhead; self-explanatory | Breaks if access rules become team-based rather than user-based |
| Cross-team table with fundamentally different filtering dimensions | Multi-column RAP with dimensional mapping table | Keeps data centralized; one policy to audit | Policy body can get complex — document and test thoroughly |
| Regulatory requirement to prove isolation (e.g., Chinese wall) | RAP + auditing via `ACCESS_HISTORY` + restricted mapping table | Demonstrable control; auditable; mapping table changes are trackable | Must restrict write access to mapping table; consider change approval workflows |

## Questions To Ask

- How many teams/tenants share the table today, and how fast is that growing?
- Do different teams need different columns, or just different rows?
- Who owns the access rules — a central security team or individual data owners?
- Is there a regulatory requirement to demonstrate data isolation?
- Do users commonly switch roles or use secondary roles in sessions?
- How often do access rules change (daily onboarding vs quarterly reorgs)?

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - RAP on Base Table vs Views with Separate RAPs]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Secondary Roles vs Composite Roles]]

## Sources To Revisit

- [Snowflake Docs: Row Access Policies](https://docs.snowflake.com/en/user-guide/security-row-intro)

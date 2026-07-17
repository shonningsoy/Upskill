---
tags:
  - note-comparison
---

# Comparison - Time Travel vs Fail-safe

> Time Travel is self-service recovery; Fail-safe is Snowflake-managed last-resort protection.

## Short Answer

Use **Time Travel** for normal recovery work: query past data, clone previous states, or undrop objects. Treat **Fail-safe** as a non-user-facing safety layer for permanent table data after Time Travel expires.

## Comparison Table

| Dimension | Time Travel | Fail-safe |
|---|---|---|
| Who uses it | Snowflake users with privileges | Snowflake/Snowflake Support |
| Primary purpose | Recent self-service recovery | Last-resort recovery for permanent data |
| SQL access | Yes: `AT`, `BEFORE`, `UNDROP`, `CLONE` | No normal user SQL workflow |
| Configurable | Yes, via retention settings | No, 7 days for permanent table data |
| Applies to transient/temporary | Limited Time Travel, no Fail-safe | No |

## Decision Rules

- If the issue is recent and inside retention, use Time Travel first.
- If Time Travel has expired, do not promise easy recovery; Fail-safe is not a standard restore button.
- Use permanent tables for data where Fail-safe protection has value.
- Use transient/temporary tables for data that is safely rebuildable.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/06 Cost Management and Operations/45 Account Usage Views]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Daily Load Overwrote Good Data]]

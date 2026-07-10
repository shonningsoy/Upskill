---
tags:
  - note-comparison
---

# Comparison - Account Usage Views vs Information Schema

> Account Usage is the delayed historical/account-wide lens; Information Schema is the fresher current/database-local metadata lens.

## Short Answer

Use **Account Usage** when the client needs historical usage, cost, audit, dropped object visibility, or account-wide operations analysis. Use **Information Schema** when the client needs current metadata or recent activity quickly inside a database context.

## Comparison Table

| Dimension | Account Usage | Information Schema |
|---|---|---|
| Location | `SNOWFLAKE.ACCOUNT_USAGE` | `<database>.INFORMATION_SCHEMA` and table functions |
| Main purpose | Historical account metadata and usage | Current metadata and recent operational introspection |
| Latency | Delayed, often 45 minutes to 3 hours depending on view | Generally no latency for many metadata views/functions |
| Retention | Commonly up to 365 days for many views | Varies, often shorter; some table functions only recent days |
| Dropped objects | Includes records for dropped objects in many views | Generally current objects only |
| Scope | Account-level historical view | Database-local metadata plus selected account/session functions |
| Cost diagnosis | Strong fit | Limited fit |
| Immediate object check | Weaker because of latency | Strong fit |
| Consultant shorthand | "What happened over time?" | "What exists or just happened?" |

## Decision Rules

- If the client asks "What changed over the last quarter?", start with Account Usage.
- If the client asks "Does this table exist right now?", start with Information Schema or `SHOW`.
- If the client asks "Why did costs increase?", start with Account Usage and Organization Usage.
- If the client asks for query history beyond the last few days, use Account Usage `QUERY_HISTORY`.
- If the client needs immediate metadata inside one database, use Information Schema.
- If dropped objects matter for cost, retention, or audit, use Account Usage.
- Always account for latency before claiming Account Usage data is complete for "right now."

## Common Misreads

- **"Account Usage is always better."** It is richer historically, but delayed.
- **"Information Schema is enough for cost analysis."** It is usually too short-lived and narrow.
- **"Query history is one thing."** Account Usage `QUERY_HISTORY` and Information Schema `QUERY_HISTORY` table functions differ in retention and usage.
- **"Missing row means missing object."** In Account Usage, latency can explain missing recent changes.

## Related Learning Topics

- [[01 Snowflake/06 Cost Management and Operations/32 Account Usage Views]]
- [[01 Snowflake/06 Cost Management and Operations/31 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/34 Budgets]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Diagnosing Snowflake Spend Increases]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Diagnosing Slow Snowflake Queries]]

## Sources To Revisit

- [Snowflake Docs: Account Usage](https://docs.snowflake.com/en/sql-reference/account-usage)
- [Snowflake Docs: Snowflake Information Schema](https://docs.snowflake.com/en/sql-reference/info-schema)
- [Snowflake Docs: QUERY_HISTORY Account Usage view](https://docs.snowflake.com/en/sql-reference/account-usage/query_history)
- [Snowflake Docs: WAREHOUSE_METERING_HISTORY Account Usage view](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_metering_history)

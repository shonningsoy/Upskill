---
status: seed
platform: Snowflake
area: Core Architecture and Concepts
topic_number: 03
tags:
  - snowflake
  - sf-core-architecture
  - learning
---

# Time Travel and Fail-safe

> Historical data recovery in Snowflake. Consultant lens: Know when teams can self-restore with SQL, when Snowflake-managed Fail-safe applies, and how retention choices affect storage cost.

## Executive Summary

- **What it is:** Time Travel lets users query, clone, or restore previous object/data states within a configured retention window. Fail-safe is a separate Snowflake-managed last-resort recovery layer for permanent table data after Time Travel expires.
- **Why it matters:** It protects teams from accidental drops, deletes, bad loads, and overwrite mistakes without immediately needing external backups or support escalation.
- **Mental model:** Time Travel is the customer-accessible "oops window"; Fail-safe is Snowflake's last-resort safety layer, not a normal restore button.
- **Best used when:** Production tables need short-term recovery, auditing, incident response, or safe point-in-time cloning.
- **Avoid or reconsider when:** A team wants Time Travel to be the main historical analytics strategy, or applies long retention to rebuildable staging data without a cost/recovery reason.

## What It Can Do

- Query historical table data using `AT` or `BEFORE`.
- Restore dropped databases, schemas, and tables using `UNDROP` if still inside the retention window.
- Clone a database, schema, or table from a previous point in time for investigation or repair.
- Set retention defaults at account, database, or schema level, then override specific objects when needed.
- Provide fast self-service recovery for common incidents such as accidental drops or bad loads.

## What It Cannot Do

- Keep unlimited history after the configured retention period expires.
- Replace deliberate historical modeling, audit tables, snapshots, or SCD patterns.
- Make Fail-safe directly queryable or self-service for users.
- Avoid storage cost when data changes frequently.
- Protect transient or temporary tables with Fail-safe.

## Core Concepts

| Concept                       | Meaning                                                                                           | Why it matters                                          |
| ----------------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Time Travel                   | Configurable historical retention window for querying, cloning, and restoring past object states. | Main self-service recovery tool for recent mistakes.    |
| Fail-safe                     | Non-configurable Snowflake-managed recovery period after Time Travel for permanent table data.    | Last-resort protection, not normal user workflow.       |
| Retention period              | Number of days historical data remains accessible through Time Travel.                            | Drives recovery capability and storage cost.            |
| `DATA_RETENTION_TIME_IN_DAYS` | Object/account parameter controlling Time Travel retention.                                       | Can be set broadly or overridden for critical objects.  |
| Permanent table               | Table type that supports Fail-safe after Time Travel.                                             | Best for important data that needs stronger protection. |
| Transient / temporary table   | Table types with reduced protection and no Fail-safe.                                             | Useful for rebuildable staging/intermediate data.       |
| Data churn                    | Amount of data changed, deleted, merged, or replaced.                                             | Main driver of Time Travel storage overhead.            |

## How It Works (Simple Flow)

1. A table is updated, deleted from, truncated, replaced, or dropped.
2. Snowflake preserves the historical state needed for Time Travel during the configured retention window.
3. Users can query past data with `AT` / `BEFORE`, restore dropped objects with `UNDROP`, or clone from an earlier state.
4. If the object inherits retention from its schema/database/account, that inherited value applies unless the object has an override.
5. When Time Travel expires, user-accessible historical recovery ends.
6. For permanent table data, Snowflake-managed Fail-safe may apply for 7 days after Time Travel.
7. For transient and temporary tables, there is no Fail-safe; expired historical data is removed rather than entering Fail-safe.

## Visuals

![[00 Home/assets/snowflake-core-03-time-travel-failsafe.png]]

- Original local diagram based on Snowflake's Time Travel, Fail-safe, and storage-cost documentation.

## Readable Snippets

```sql
-- Query a table as it looked at a specific time.
select *
from orders at (timestamp => '2026-05-27 09:00:00'::timestamp);
```

```sql
-- Query data before a specific statement changed it.
select *
from orders before (statement => '01b12345-0001-abcd-0000-000000000000');
```

```sql
-- Restore a dropped table if it is still inside the Time Travel window.
undrop table orders;
```

```sql
-- Safer recovery pattern: clone the earlier state, inspect it, then decide what to merge back.
create table orders_recovery
clone orders
  at (offset => -3600);
```

```sql
-- Set retention at a schema level so new tables inherit the default.
alter schema finance
  set data_retention_time_in_days = 30;

-- Override one critical table.
alter table finance.transactions
  set data_retention_time_in_days = 90;
```

## Consultant Talking Points

- **Client question this answers:** How quickly can we recover from accidental deletes, drops, bad loads, or overwrite mistakes?
- **Trade-offs to mention:** Longer retention improves recovery options but can increase storage cost, especially for high-churn tables.
- **Risk or governance angle:** Time Travel reduces incident impact, but it does not replace RBAC, change controls, backups, or intentional audit/history design.
- **Cost/performance angle:** Time Travel storage is not seven full table copies for seven days; cost is driven mainly by retained historical versions of changed data.

## Common Pitfalls

- Treating Fail-safe as a normal SQL-accessible restore feature.
- Using long retention on every schema/table instead of matching retention to recovery value.
- Keeping large daily rebuilt staging tables as permanent tables with long retention by default.
- Confusing Time Travel with a proper business-history strategy.
- Assuming retention settings always need to be table-by-table instead of using account/database/schema defaults carefully.
- Forgetting that reducing retention later does not necessarily change already-dropped child objects the way teams expect.

## When to Recommend What (Decision Table)

| Situation                                              | Recommend                                                     | Why                                                         | Watch-outs                                                   |
| ------------------------------------------------------ | ------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| Accidental table drop discovered quickly               | Use `UNDROP` through Time Travel                              | Fast self-service recovery inside retention window          | Must act before retention expires                            |
| Bad load or overwrite                                  | Clone/query the prior state with `AT` or `BEFORE`             | Safer than immediately overwriting production again         | Need timestamp, offset, or query ID                          |
| Critical production finance/customer data              | Permanent tables with longer selective retention              | Recovery and audit value can justify storage cost           | Longer than 1 day requires Enterprise Edition or higher      |
| Rebuildable staging/intermediate data                  | Transient table or short retention                            | Avoids paying for protection where source can be reloaded   | Keep enough retention for realistic bad-load detection       |
| Client wants 90 days everywhere                        | Classify data by recovery value first                         | Prevents broad storage cost from low-value objects          | Global/schema defaults can affect many future objects        |
| Client needs historical analytics                      | Model history intentionally with snapshots/audit tables       | Time Travel is for recovery, not long-term business history | Extra design work, storage, and governance needed            |
| High-churn table with frequent `MERGE`/`UPDATE`/reload | Measure Time Travel storage impact before extending retention | Churn can retain more historical data                       | Cost may surprise teams even if current table size is stable |

## Related Topics

- [[01 Snowflake/01 Core Architecture and Concepts/Core Architecture and Concepts Overview]]
- [[01 Snowflake/01 Core Architecture and Concepts/02 Micro-partitions and Clustering]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/06 Cost Management and Operations/31 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/32 Account Usage Views]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Fail-safe]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Permanent vs Transient vs Temporary Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Modeled Historical Data]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing Table Retention by Data Criticality]]

## Questions

- Which schemas contain critical data that deserves longer retention?
- Which staging/intermediate tables are rebuildable and should use transient tables or shorter retention?
- How quickly would the team notice a bad load, and does the retention window match that detection time?
- Does the client need recovery history, or true analytical history that should be modeled separately?

## Sources To Revisit

- Snowflake docs: Understanding and using Time Travel - https://docs.snowflake.com/en/user-guide/data-time-travel
- Snowflake docs: Understanding and viewing Fail-safe - https://docs.snowflake.com/en/user-guide/data-failsafe
- Snowflake docs: Working with Temporary and Transient Tables - https://docs.snowflake.com/en/user-guide/tables-temp-transient
- Snowflake docs: Storage costs for Time Travel and Fail-safe - https://docs.snowflake.com/en/user-guide/data-cdp-storage-costs

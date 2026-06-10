---
status: active
platform: Snowflake
area: Security and Governance
topic_number: 13
tags:
  - snowflake
  - sf-security-governance
  - learning
---

# Row Access Policies

> Schema-level policy objects that dynamically filter rows at query time based on the querying user's role or context. Consultant lens: enforce data segregation (regional restrictions, Chinese walls, multi-tenant isolation) without maintaining separate views or copies.

## Executive Summary

- **What it is:** A boolean expression attached to a table/view that Snowflake evaluates per row at query time — `true` = row visible, `false` = row silently filtered out.
- **Why it matters:** Lets you enforce row-level data segregation centrally, independent of which tool, query, or BI layer accesses the table.
- **Mental model:** A centrally managed, invisible WHERE clause that the querying user cannot bypass or see. RBAC is the door lock to the room; RAP decides which drawers open for you once you're inside.
- **Best used when:** Multiple teams/tenants share the same table but should see different subsets of rows (regional, departmental, customer-based).
- **Avoid or reconsider when:** You need column-level restrictions (use masking policies), row-level write control (RAPs are read-only), or the filtering logic is so simple that RBAC + views already solves it cleanly.

## What It Can Do

- Filter rows dynamically per user, role, or session context at query time.
- Reference `CURRENT_ROLE()`, `CURRENT_USER()`, `IS_ROLE_IN_SESSION()`, or lookup mapping tables for flexible logic.
- Attach the same policy to multiple tables with different column names (parameter binding is positional, not name-based).
- Stack with Column-level Masking Policies on the same table (RAP evaluates first, masking second).
- Attach to views, not just base tables.
- Decouple policy ownership from table ownership — enables centralized security governance.
- Accept multiple column arguments for multi-dimensional filtering.

## What It Cannot Do

- Restrict specific columns — that's Column-level Masking Policies.
- Produce an error or alert — filtered rows are silently hidden; users don't know rows were removed.
- Be overridden by the querying user — by design, but makes debugging "missing data" harder.
- Control writes — RAPs only filter reads; they don't prevent inserts/updates to restricted rows.
- Inherit role hierarchy in the policy body — `CURRENT_ROLE()` returns the literal active role, not child roles.
- Stack multiple policies per table — one RAP per table/view at a time.

## Core Concepts

| Concept | Meaning | Why it matters |
|---|---|---|
| Policy body | A SQL expression returning BOOLEAN, evaluated per row | The filtering logic lives here — keep it readable |
| Parameter binding (`ON`) | Maps real table columns to the policy's positional parameters | Decouples policy from column names — enables reuse across tables |
| Mapping table | A lookup table (role → allowed values) queried inside the policy | Governance lever — change access by updating rows, not altering policy DDL |
| `CURRENT_ROLE()` | Returns the literal active role name at query time | RAP checks this — not the role hierarchy. Parent roles need explicit entries |
| `IS_ROLE_IN_SESSION()` | Returns true if a role is active (primary or secondary) | More flexible than `CURRENT_ROLE()` when secondary roles are enabled |
| One-policy-per-table rule | Only one RAP can be attached to a given table/view | Forces you to design one comprehensive policy — can get complex for cross-team tables |
| Silent filtering | Excluded rows return no error — user just sees fewer rows | Clean UX but harder to debug; test with `EXECUTE AS` |

## How It Works (Simple Flow)

1. You create a Row Access Policy with named parameters and a boolean expression body.
2. You attach the policy to a table using `ALTER TABLE ... ADD ROW ACCESS POLICY ... ON (column1, column2)`.
3. The `ON (columns)` maps real table columns to the policy's positional parameters.
4. At query time, Snowflake first checks RBAC (can this role access the table at all?).
5. If RBAC passes, the policy is evaluated for every row — each row's column values are passed into the policy parameters.
6. The policy body runs (typically checking `CURRENT_ROLE()` against a mapping table).
7. Rows returning `true` are included in results; `false` rows are silently excluded.
8. Column masking (if any) is applied to the surviving rows before returning results.

## Visuals

No high-value visual identified for this topic yet.

## Readable Snippets

### Basic pattern: mapping table + policy + attach

```sql
-- 1. Mapping table: which role sees which region
CREATE TABLE security.region_access (
    role_name STRING,
    allowed_region STRING
);

INSERT INTO security.region_access VALUES
    ('EMEA_ANALYST', 'EMEA'),
    ('APAC_ANALYST', 'APAC'),
    ('GLOBAL_ADMIN', 'ALL');

-- 2. Create the policy (parameter name is arbitrary — binding is positional)
CREATE OR REPLACE ROW ACCESS POLICY security.region_filter
AS (region_val STRING) RETURNS BOOLEAN ->
    CURRENT_ROLE() IN ('ACCOUNTADMIN', 'SYSADMIN')
    OR EXISTS (
        SELECT 1 FROM security.region_access
        WHERE role_name = CURRENT_ROLE()
          AND (allowed_region = region_val OR allowed_region = 'ALL')
    );

-- 3. Attach to table — ON (region) maps the column to region_val
ALTER TABLE sales.orders
    ADD ROW ACCESS POLICY security.region_filter ON (region);
```

### Reusing the same policy on tables with different column names

```sql
-- Same policy, different column name on each table
ALTER TABLE campaigns ADD ROW ACCESS POLICY security.team_filter ON (team);
ALTER TABLE spend ADD ROW ACCESS POLICY security.team_filter ON (department);
-- Both pass their column value into the policy's first parameter
```

### Multi-column policy for cross-team tables

```sql
-- Policy with two parameters for multi-dimensional filtering
CREATE OR REPLACE ROW ACCESS POLICY security.leads_filter
AS (source_val STRING, assigned_team_val STRING) RETURNS BOOLEAN ->
    CURRENT_ROLE() IN ('ACCOUNTADMIN', 'SYSADMIN')
    OR EXISTS (
        SELECT 1 FROM security.leads_access
        WHERE role_name = CURRENT_ROLE()
          AND filter_column = 'ALL'
    )
    OR EXISTS (
        SELECT 1 FROM security.leads_access
        WHERE role_name = CURRENT_ROLE()
          AND (
              (filter_column = 'source' AND allowed_value = source_val)
              OR (filter_column = 'assigned_team' AND allowed_value = assigned_team_val)
          )
    );

-- Attach with two columns mapped positionally
ALTER TABLE shared.leads
    ADD ROW ACCESS POLICY security.leads_filter ON (source, assigned_team);
```

## Consultant Talking Points

- **Client question this answers:** "How do we let multiple teams query the same table but only see their own data — without maintaining separate copies or views?"
- **Trade-offs to mention:** Silent filtering is great UX but hard to debug. Mapping tables are operationally clean but become critical governance artifacts that must be audited. One-policy-per-table forces comprehensive policy design up front.
- **Risk or governance angle:** Policy ownership can be decoupled from table ownership — powerful for centralized security teams. The mapping table is the single source of truth; changes there change who sees what. Audit via `ACCESS_HISTORY`.
- **Cost/performance angle:** Policy evaluation runs per query. If the policy subqueries a mapping table, that table gets scanned frequently — keep it small and clustered. Complex policy logic adds latency to every query on the table.

## Common Pitfalls

- **Parent roles don't inherit RAP access** — `CURRENT_ROLE()` returns the literal active role. A manager role inheriting from child roles still needs its own mapping entry, or users see nothing.
- **Forgetting the admin bypass** — if `ACCOUNTADMIN` isn't explicitly exempted in the policy body, even admins get filtered. RAPs are that strong.
- **Debugging "missing data" without knowing RAPs exist** — users see zero rows and no error. Document which tables have RAPs and use `EXECUTE AS` for testing.
- **Mapping table becomes ungoverned** — changes to the mapping table change access silently. Treat it as a security artifact: restrict write access, enable change tracking.
- **Secondary roles complicate `CURRENT_ROLE()`** — if users activate secondary roles, `CURRENT_ROLE()` still returns only the primary role. Use `IS_ROLE_IN_SESSION()` for multi-role awareness.
- **One policy per table limits composability** — you can't layer multiple simple policies; you must consolidate all logic into one (potentially complex) policy body.

## When to Recommend What (Decision Table)

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Single table, multiple teams, same columns needed | Single RAP with mapping table on base table | One governance artifact, no view maintenance | Policy logic complexity grows with team count |
| Cross-team table with different filtering dimensions | Single RAP with multi-column parameters | Keeps data in one place, one policy to audit | Policy body can get hard to read — document it well |
| Teams need different column projections AND different rows | Separate views with simpler RAPs per view | Cleaner policies, column control included | Must lock down base table access; views must stay in sync |
| Very simple "own data only" filter | RAP with `CURRENT_USER()` or `CURRENT_ROLE()` check | No mapping table needed, minimal overhead | Doesn't scale if access rules become team-based later |
| Row filtering + column masking needed together | RAP + Masking Policy on same table | They stack naturally (RAP first, masking second) | One of each per table — plan the combined logic upfront |

## RBAC + RAP Interaction Pattern

RBAC and RAP are two independent layers:

| Layer | Controls | Granularity | Failure mode |
|---|---|---|---|
| RBAC | Can you access this object at all? | Object-level (all-or-nothing) | Hard error: "access denied" |
| RAP | Which rows do you see once inside? | Row-level (conditional) | Silent: zero rows, no error |

**Evaluation order:** RBAC check → RAP evaluation → Column masking → Results returned.

RBAC inheritance (parent roles inherit child privileges) does NOT flow into RAP mapping tables. They are separate systems.

## Related Topics

- [[01 Snowflake/03 Security and Governance/Security and Governance Overview]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]
- [[01 Snowflake/03 Security and Governance/15 Data Classification]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - RAP on Base Table vs Views with Separate RAPs]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Row-Level Data Isolation Strategy]]

## Questions

- How does RAP performance degrade with very large mapping tables (10k+ entries)?
- Can RAPs reference Secure UDFs for more complex access logic?
- How do RAPs interact with data sharing — does the consumer's role context work across accounts?

## Sources To Revisit

- [Snowflake Docs: Row Access Policies](https://docs.snowflake.com/en/user-guide/security-row-intro)
- [Snowflake Docs: CREATE ROW ACCESS POLICY](https://docs.snowflake.com/en/sql-reference/sql/create-row-access-policy)

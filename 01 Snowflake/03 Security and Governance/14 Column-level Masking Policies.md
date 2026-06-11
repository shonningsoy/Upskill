---
status: active
platform: Snowflake
area: Security and Governance
topic_number: 14
tags:
  - snowflake
  - sf-security-governance
  - learning
---

# Column-level Masking Policies

> Schema-level policy objects that dynamically transform sensitive column values at query time based on the querying user's role or context. Consultant lens: supports PII/PCI compliance without duplicating data — everyone queries the same table, privileged roles see real values, others see masked output.

## Executive Summary

- **What it is:** A SQL expression attached to a column that transforms the value at query time — returning the real value, a partial mask, null, or a hash depending on who's asking.
- **Why it matters:** Lets you serve multiple audiences from one table without maintaining sanitized copies. Analysts see masked PII, stewards see real values, same source of truth.
- **Mental model:** A conditional CASE expression injected into every SELECT on that column, controlled centrally and invisibly. RAP is the row filter; masking is the column filter. Together they give fine-grained control without data duplication.
- **Best used when:** Tables contain PII, PCI, salary, health data, or other sensitive columns that some roles must see and others must not.
- **Avoid or reconsider when:** You need to hide entire rows (use RAP), control writes (masking is read-only), or the masking would break critical downstream aggregations without a viable alternative pattern.

## What It Can Do

- Dynamically transform column values per role/user at query time (null, redact, partial mask, hash).
- Attach the same reusable policy to columns on different tables (binding is positional, same as RAP).
- Stack with Row Access Policies on the same table (RAP evaluates first, masking second).
- Support conditional masking using additional columns from the same row via the `USING` clause.
- Decouple policy ownership from table ownership — centralized security team can control all masking.
- Attach to view columns, not just base tables.
- Preserve join/grouping capability when using hash-based masking (SHA2).

## What It Cannot Do

- Hide entire rows — that's Row Access Policies.
- Change the output data type — must return the same type as the column (can't mask a NUMBER to a STRING).
- Prevent writes — users with INSERT/UPDATE privileges write raw values; masking only applies on SELECT.
- Preserve aggregation accuracy with naive masking — full-null breaks `COUNT(column)`, constant-redact breaks `COUNT(DISTINCT)`.
- Easily reference other columns — requires the `USING` clause with additional column arguments (adds complexity).
- Stack multiple policies per column — one masking policy per column at a time.

## Core Concepts

| Concept | Meaning | Why it matters |
|---|---|---|
| Policy body | A SQL expression that receives the column value and returns a (possibly transformed) value of the same type | The masking logic lives here — keep it readable |
| Return type constraint | Policy must return the same data type as the column it's attached to | Can't mask a NUMBER into a STRING — plan per-type policies |
| `USING` clause | Maps additional row columns into the policy as extra arguments | Enables conditional masking based on other column values (e.g., department) |
| Reusability | Same policy can attach to STRING columns across many tables | Design "standard" policies per type: `string_full_mask`, `string_email_mask`, `number_null_mask` |
| One-policy-per-column rule | Each column gets at most one masking policy | Unlike RAP (one per table), you can mask many columns on one table — each with its own policy |
| Evaluation order | RAP filters rows first → masking transforms surviving rows' columns → results returned | No point masking values on rows the user won't see |

## Common Masking Patterns

| Pattern | Example output | Use case | Aggregation impact |
|---|---|---|---|
| Full null | `NULL` | Default safe — analysts can still count rows | Breaks `COUNT(column)` (nulls excluded) |
| Full redact | `'***MASKED***'` | Shows "something is here" | Breaks `COUNT(DISTINCT)` — always returns 1 |
| Partial mask | `'XXX-XX-1234'` | Customer service needs partial identification | Preserves uniqueness for last-N digits |
| Hash/tokenize | `SHA2(value)` | Preserves joins/grouping without revealing data | Preserves cardinality — best for analytics |
| Conditional full | Real value for privileged role, null for others | Simplest two-tier model | Depends on tier |

## How It Works (Simple Flow)

1. You create a masking policy with a parameter (the column value) and a body that returns a transformed value of the same type.
2. You attach the policy to a column using `ALTER TABLE ... MODIFY COLUMN ... SET MASKING POLICY ...`.
3. Optionally, use `USING (column, other_column)` to pass additional row columns into the policy.
4. At query time, Snowflake first checks RBAC (can this role access the table?).
5. If a RAP exists, it filters rows (row-level check).
6. For surviving rows, the masking policy evaluates per row: checks `CURRENT_ROLE()` or mapping table, returns real or masked value.
7. The user sees all permitted rows, but sensitive columns show transformed values based on their role.

## Visuals

No high-value visual identified for this topic yet.

## Readable Snippets

### Basic pattern: role-based email masking

```sql
-- Create a masking policy for email columns
CREATE OR REPLACE MASKING POLICY security.email_mask
AS (val STRING) RETURNS STRING ->
    CASE
        WHEN CURRENT_ROLE() IN ('ACCOUNTADMIN', 'DATA_STEWARD') THEN val
        WHEN CURRENT_ROLE() IN ('ANALYST_SENIOR') THEN REGEXP_REPLACE(val, '.+@', '****@')
        ELSE '***MASKED***'
    END;

-- Attach to a column
ALTER TABLE customers.profiles
    MODIFY COLUMN email SET MASKING POLICY security.email_mask;

-- Results:
-- DATA_STEWARD sees:    john.smith@acme.com
-- ANALYST_SENIOR sees:  ****@acme.com
-- Everyone else sees:   ***MASKED***
```

### Hash-based masking (preserves cardinality for analytics)

```sql
CREATE OR REPLACE MASKING POLICY security.string_hash_mask
AS (val STRING) RETURNS STRING ->
    CASE
        WHEN CURRENT_ROLE() IN ('ACCOUNTADMIN', 'DATA_STEWARD') THEN val
        ELSE SHA2(val)
    END;

-- COUNT(DISTINCT email) still returns correct cardinality
-- JOINs on masked columns still work (same input → same hash)
```

### Conditional masking with additional columns (USING clause)

```sql
-- Mask salary unless viewer is in HR or same department
CREATE OR REPLACE MASKING POLICY security.salary_mask
AS (salary_val NUMBER, dept_val STRING) RETURNS NUMBER ->
    CASE
        WHEN CURRENT_ROLE() IN ('HR_ADMIN', 'ACCOUNTADMIN') THEN salary_val
        WHEN dept_val = 'FINANCE' THEN salary_val
        ELSE NULL
    END;

-- Attach with USING to pass the department column as second argument
ALTER TABLE hr.employees
    MODIFY COLUMN salary SET MASKING POLICY security.salary_mask
    USING (salary, department);
```

### Reusing the same policy across tables

```sql
-- One policy, many columns
ALTER TABLE customers.profiles
    MODIFY COLUMN email SET MASKING POLICY security.string_full_mask;

ALTER TABLE orders.contacts
    MODIFY COLUMN contact_email SET MASKING POLICY security.string_full_mask;

-- Same policy, different column names — binding is positional
```

## Consultant Talking Points

- **Client question this answers:** "How do we let analysts query PII tables for metrics without exposing raw sensitive data — and without maintaining separate sanitized copies?"
- **Trade-offs to mention:** Masking pattern choice affects downstream aggregation accuracy. Full-null is safest but breaks counts. Hash preserves cardinality but looks ugly in reports. Partial mask is user-friendly but leaks some information. Match pattern to use case.
- **Risk or governance angle:** Policy ownership is separate from table ownership — security team can own all masking centrally. Combined with Data Classification (topic 15), you can auto-discover sensitive columns and systematically apply policies. Audit who has unmasked access.
- **Cost/performance angle:** Minimal overhead for simple CASE logic. UDF-based policies or policies with subqueries against mapping tables add per-row-per-column cost. For high-volume tables with many masked columns, test query latency.

## Common Pitfalls

- **Aggregation breakage** — `COUNT(DISTINCT email)` returns 1 if everyone gets the same `'***MASKED***'` constant. Use hash-based masking when analytics accuracy matters.
- **BI tool display issues** — some tools (Tableau, Power BI) may error or display oddly when they expect real values and get redacted strings or unexpected nulls. Test with the client's actual stack.
- **Return type mismatch** — policy must return the same type as the column. Trying to mask a NUMBER column to return `'REDACTED'` (string) fails at creation time. Plan per-type standard policies.
- **Forgetting USING for conditional logic** — without `USING`, the policy only sees the masked column value and session context. If you need to check another column (e.g., department), you must add it explicitly.
- **Write path unaffected** — masking only applies to SELECT. Users with INSERT/UPDATE privileges still write raw values. Don't confuse masking with input sanitization.
- **One policy per column, but many columns per table** — different from RAP (one per table). You can attach different policies to different columns, but planning the overall masking strategy per table is still important.

## When to Recommend What (Decision Table)

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Analysts need row counts but not raw PII | Full-null masking | Simple, safe, preserves row-level counts via `COUNT(*)` | Breaks `COUNT(column)` — educate users |
| Downstream joins/grouping must still work | Hash-based masking (SHA2) | Preserves cardinality and joinability | Output looks like gibberish in reports — only for backend analytics |
| Customer service needs partial identification | Partial mask (last-4, domain-only) | User-friendly, still useful for identification | Leaks partial information — ensure residual data isn't re-identifiable |
| Simple privileged/unprivileged split | Conditional full (CASE on CURRENT_ROLE) | Minimal logic, easy to audit | Doesn't scale well if you need 3+ masking tiers |
| Multiple sensitivity tiers (full, partial, none) | Mapping-table-driven policy | Flexible, no DDL changes when roles change | Mapping table becomes governance artifact — audit it |
| Department-specific visibility (e.g., salary) | Conditional masking with USING clause | Enables row-context-aware decisions | Adds complexity; document the USING bindings clearly |

## RAP vs Masking: Complementary Layers

| Dimension | Row Access Policy | Column Masking Policy |
|---|---|---|
| What it hides | Entire rows | Individual column values |
| User sees | Fewer rows, no error | All rows, but some values are transformed |
| Evaluation order | First | Second (after RAP filters rows) |
| Limit | One per table | One per column (many columns maskable per table) |
| Return | Boolean (include/exclude) | Transformed value (same data type) |
| Use case | "Don't see other regions' data" | "Don't see raw SSN/email/salary" |

## Related Topics

- [[01 Snowflake/03 Security and Governance/Security and Governance Overview]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/15 Data Classification]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Row-Level Data Isolation Strategy]]

## Questions

- How do masking policies interact with CLONE operations — does the cloned table inherit policies?
- Can masking policies reference external functions or Snowpark UDFs for tokenization?
- What happens to masked columns in CTAS (CREATE TABLE AS SELECT) — does the new table get real or masked values?

## Sources To Revisit

- [Snowflake Docs: Dynamic Data Masking](https://docs.snowflake.com/en/user-guide/security-column-ddm-intro)
- [Snowflake Docs: CREATE MASKING POLICY](https://docs.snowflake.com/en/sql-reference/sql/create-masking-policy)

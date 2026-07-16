---
status: hub
platform: dbt
area: Packages Macros and Advanced Reuse
tags:
  - dbt
  - dbt-packages-macros
  - map
---

# Packages Macros and Advanced Reuse Overview

> Reusable dbt logic: packages, Jinja, macros, tests, dispatch, hooks, operations, and abstraction boundaries.

## Topics

- [[02 dbt/07 Packages Macros and Advanced Reuse/60 Package Fundamentals|60 - Package Fundamentals]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/61 packages.yml vs dependencies.yml|61 - packages.yml vs dependencies.yml]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/62 Must-Have Utility Packages|62 - Must-Have Utility Packages]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/63 Data Quality Packages|63 - Data Quality Packages]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/64 Snowflake and Operations Packages|64 - Snowflake and Operations Packages]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/65 Finance and Bank-Relevant Packages|65 - Finance and Bank-Relevant Packages]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/66 Package Governance|66 - Package Governance]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/67 Jinja Fundamentals|67 - Jinja Fundamentals]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/68 Macros as Reusable SQL Functions|68 - Macros as Reusable SQL Functions]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/69 Custom Generic Tests|69 - Custom Generic Tests]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/70 Adapter Dispatch|70 - Adapter Dispatch]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/71 Hooks and Operations|71 - Hooks and Operations]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/72 Advanced Macro Boundaries|72 - Advanced Macro Boundaries]]

## Topic Summaries

### [[02 dbt/07 Packages Macros and Advanced Reuse/60 Package Fundamentals|60 - Package Fundamentals]]

Explains packages as reusable dbt projects containing macros, models, tests, and resources.

### [[02 dbt/07 Packages Macros and Advanced Reuse/61 packages.yml vs dependencies.yml|61 - packages.yml vs dependencies.yml]]

Distinguishes ordinary package installation from dbt Mesh-style project dependencies.

### [[02 dbt/07 Packages Macros and Advanced Reuse/62 Must-Have Utility Packages|62 - Must-Have Utility Packages]]

Covers dbt_utils, codegen, audit_helper, and when they save real time.

### [[02 dbt/07 Packages Macros and Advanced Reuse/63 Data Quality Packages|63 - Data Quality Packages]]

Covers dbt_expectations, elementary, dbt_project_evaluator, and when to avoid test sprawl.

### [[02 dbt/07 Packages Macros and Advanced Reuse/64 Snowflake and Operations Packages|64 - Snowflake and Operations Packages]]

Covers dbt_snowflake_monitoring, query-tag packages, dbt_external_tables, and Snowflake-specific caveats.

### [[02 dbt/07 Packages Macros and Advanced Reuse/65 Finance and Bank-Relevant Packages|65 - Finance and Bank-Relevant Packages]]

Evaluates Data Vault packages, constraints, audit packages, observability, metadata testing, and package approval risk.

### [[02 dbt/07 Packages Macros and Advanced Reuse/66 Package Governance|66 - Package Governance]]

Covers version pinning, Fusion compatibility, package review, transitive dependencies, support boundaries, and regulated-environment approval.

### [[02 dbt/07 Packages Macros and Advanced Reuse/67 Jinja Fundamentals|67 - Jinja Fundamentals]]

Explains templating, variables, loops, conditionals, whitespace control, and compilation.

### [[02 dbt/07 Packages Macros and Advanced Reuse/68 Macros as Reusable SQL Functions|68 - Macros as Reusable SQL Functions]]

Teaches how to remove repetition without hiding business logic.

### [[02 dbt/07 Packages Macros and Advanced Reuse/69 Custom Generic Tests|69 - Custom Generic Tests]]

Shows how macros become reusable data quality checks.

### [[02 dbt/07 Packages Macros and Advanced Reuse/70 Adapter Dispatch|70 - Adapter Dispatch]]

Explains warehouse-specific behavior behind a common macro interface.

### [[02 dbt/07 Packages Macros and Advanced Reuse/71 Hooks and Operations|71 - Hooks and Operations]]

Covers on-run-start, on-run-end, grants, audit logging, and dbt run-operation.

### [[02 dbt/07 Packages Macros and Advanced Reuse/72 Advanced Macro Boundaries|72 - Advanced Macro Boundaries]]

Teaches when abstraction becomes harmful: unreadable compiled SQL, hidden dependencies, and hard-to-debug behavior.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/Governance Semantic Layer and Mesh Overview|Previous: Governance Semantic Layer and Mesh]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/dbt on Snowflake and Finance Patterns Overview|Next: dbt on Snowflake and Finance Patterns]]

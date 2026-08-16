# dbt Learning Map

Use this as the main entry point for dbt after the Snowflake foundation is in place.

The graph should follow the same general pattern as Snowflake:

`dbt Learning Map -> area overview -> individual topic notes`

Keep the dbt notes primarily about dbt. Link to Snowflake only where Snowflake changes the practical recommendation: compute cost, RBAC, schemas, materializations, orchestration, monitoring, or deployment.

## Suggested Path

1. Foundation: understand what dbt is, what it is not, and how projects, models, `ref()`, `source()`, targets, and artifacts fit together.
2. Modeling: learn how dbt teams structure staging, intermediate, marts, dimensions, facts, snapshots, and reconciliation models.
3. Quality: learn tests, documentation, source freshness, exposures, contracts, and audit controls.
4. Performance: learn materializations, incremental models, microbatching, model selection, state, deferral, and Snowflake cost implications.
5. Operations: learn environments, jobs, CI/CD, artifacts, orchestration, secrets, package governance, and incident response.
6. Governance: learn ownership, access, model versions, dbt Mesh, semantic models, metrics, and regulated-domain boundaries.
7. Reuse: learn packages, Jinja, macros, custom tests, dispatch, hooks, and when abstraction helps or hurts.
8. Enterprise Snowflake and finance context: apply dbt to Snowflake, investment-bank data domains, controls, lineage, and cost governance.

## Area Hubs

- [[02 dbt/01 Core Concepts and Project Structure/Core Concepts and Project Structure Overview|01 - Core Concepts and Project Structure]]
- [[02 dbt/02 Modeling Patterns and Layering/Modeling Patterns and Layering Overview|02 - Modeling Patterns and Layering]]
- [[02 dbt/03 Testing Documentation and Data Quality/Testing Documentation and Data Quality Overview|03 - Testing Documentation and Data Quality]]
- [[02 dbt/04 Incremental Processing and Performance/Incremental Processing and Performance Overview|04 - Incremental Processing and Performance]]
- [[02 dbt/05 Deployment CI CD and Operations/Deployment CI CD and Operations Overview|05 - Deployment CI CD and Operations]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/Governance Semantic Layer and Mesh Overview|06 - Governance Semantic Layer and Mesh]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/Packages Macros and Advanced Reuse Overview|07 - Packages Macros and Advanced Reuse]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/dbt on Snowflake and Finance Patterns Overview|08 - dbt on Snowflake and Finance Patterns]]

## 01 Core Concepts and Project Structure

Main hub: [[02 dbt/01 Core Concepts and Project Structure/Core Concepts and Project Structure Overview|Core Concepts and Project Structure Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/01 Core Concepts and Project Structure/01 What dbt Is and Is Not|01 - What dbt Is and Is Not]] | Separates transformation workflow from ingestion, storage, BI, orchestration, and governance tooling. |
| [[02 dbt/01 Core Concepts and Project Structure/02 dbt Core Fusion dbt Platform and dbt Projects on Snowflake|02 - dbt Core, Fusion, dbt Platform, and dbt Projects on Snowflake]] | Helps compare execution engines and control planes without confusing where SQL actually runs. |
| [[02 dbt/01 Core Concepts and Project Structure/03 Project Anatomy and dbt_project.yml|03 - Project Anatomy and dbt_project.yml]] | Explains folders, naming, configs, model paths, and how a dbt project becomes maintainable. |
| [[02 dbt/01 Core Concepts and Project Structure/04 Environments Profiles Targets and Credentials|04 - Environments, Profiles, Targets, and Credentials]] | Critical for dev/test/prod separation, warehouse choice, secrets, and role design. |
| [[02 dbt/01 Core Concepts and Project Structure/05 Models ref source and the DAG|05 - Models, ref(), source(), and the DAG]] | Core mental model for dependency management and lineage. |
| [[02 dbt/01 Core Concepts and Project Structure/06 Commands and Artifacts|06 - Commands and Artifacts]] | Covers run, test, build, compile, deps, seed, snapshot, manifest.json, run_results.json, and catalog.json. |
| [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness|07 - Sources and Source Freshness]] | Connects raw data availability to downstream reliability. |
| [[02 dbt/01 Core Concepts and Project Structure/08 Seeds and Static Reference Data|08 - Seeds and Static Reference Data]] | Useful for small controlled mappings, but risky for sensitive or changing production data. |
| [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking|09 - Snapshots and Historical Change Tracking]] | Important for mutable source data and slowly changing dimensions. |
| [[02 dbt/01 Core Concepts and Project Structure/10 Documentation Lineage and Exposures|10 - Documentation, Lineage, and Exposures]] | Turns dbt from SQL execution into a knowledge and ownership layer. |

## 02 Modeling Patterns and Layering

Main hub: [[02 dbt/02 Modeling Patterns and Layering/Modeling Patterns and Layering Overview|Modeling Patterns and Layering Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/02 Modeling Patterns and Layering/11 Staging Models|11 - Staging Models]] | Standardizes raw source data into clean, typed, renamed, lightly transformed building blocks. |
| [[02 dbt/02 Modeling Patterns and Layering/12 Intermediate Models|12 - Intermediate Models]] | Keeps complex business logic readable without exposing every step as a consumer-facing asset. |
| [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products|13 - Marts and Data Products]] | Frames dbt output as trusted analytical products for BI, risk, finance, and regulatory use. |
| [[02 dbt/02 Modeling Patterns and Layering/14 Dimensional Modeling with dbt|14 - Dimensional Modeling with dbt]] | Gives structure for facts, dimensions, grain, keys, and conformed entities. |
| [[02 dbt/02 Modeling Patterns and Layering/15 Finance Modeling Patterns|15 - Finance Modeling Patterns]] | Covers trades, orders, executions, positions, instruments, counterparties, accounts, FX, prices, P&L, and risk measures. |
| [[02 dbt/02 Modeling Patterns and Layering/16 Naming Conventions and Folder Design|16 - Naming Conventions and Folder Design]] | Makes large projects navigable and reduces onboarding friction. |
| [[02 dbt/02 Modeling Patterns and Layering/17 Refactoring Legacy SQL into dbt|17 - Refactoring Legacy SQL into dbt]] | Highly relevant in consulting, where existing SQL estates rarely start clean. |
| [[02 dbt/02 Modeling Patterns and Layering/18 Multi-source Conformed Models|18 - Multi-source Conformed Models]] | Handles the reality of combining trading, risk, finance, reference, and market-data sources. |
| [[02 dbt/02 Modeling Patterns and Layering/19 Late-arriving Data Corrections and Restatements|19 - Late-arriving Data, Corrections, and Restatements]] | Essential in banks where adjustments and backdated corrections are normal. |
| [[02 dbt/02 Modeling Patterns and Layering/20 Reconciliation Models|20 - Reconciliation Models]] | Provides evidence that trade, position, cash, P&L, and regulatory numbers tie out. |

## 03 Testing Documentation and Data Quality

Main hub: [[02 dbt/03 Testing Documentation and Data Quality/Testing Documentation and Data Quality Overview|Testing Documentation and Data Quality Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/03 Testing Documentation and Data Quality/21 Generic Singular and Custom Data Tests|21 - Generic, Singular, and Custom Data Tests]] | Core dbt quality mechanism and the basis for consultant discussions about trust. |
| [[02 dbt/03 Testing Documentation and Data Quality/22 Unit Tests for SQL Logic|22 - Unit Tests for SQL Logic]] | Validates transformation logic on controlled inputs before expensive production builds. |
| [[02 dbt/03 Testing Documentation and Data Quality/23 Source Freshness and SLA Monitoring|23 - Source Freshness and SLA Monitoring]] | Separates "the model ran" from "the upstream data was actually current." |
| [[02 dbt/03 Testing Documentation and Data Quality/24 Documentation Blocks and Catalog|24 - Documentation Blocks and Catalog]] | Makes model and column definitions reusable and reviewable. |
| [[02 dbt/03 Testing Documentation and Data Quality/25 Exposures|25 - Exposures]] | Links dbt assets to dashboards, reports, ML jobs, regulatory outputs, and owners. |
| [[02 dbt/03 Testing Documentation and Data Quality/26 Model Contracts and Constraints|26 - Model Contracts and Constraints]] | Creates stronger producer-consumer expectations for important interfaces. |
| [[02 dbt/03 Testing Documentation and Data Quality/27 Test Severity and Failure Handling|27 - Test Severity and Failure Handling]] | Determines whether failed tests should warn, block publication, alert, or trigger incident handling. |
| [[02 dbt/03 Testing Documentation and Data Quality/28 Audit and Migration Validation|28 - Audit and Migration Validation]] | Uses row counts, checksums, and diff logic to prove refactors or migrations are safe. |
| [[02 dbt/03 Testing Documentation and Data Quality/29 Data Quality Strategy in Regulated Environments|29 - Data Quality Strategy in Regulated Environments]] | Connects tests to control evidence, ownership, sign-off, and operational accountability. |

## 04 Incremental Processing and Performance

Main hub: [[02 dbt/04 Incremental Processing and Performance/Incremental Processing and Performance Overview|Incremental Processing and Performance Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/04 Incremental Processing and Performance/30 Materializations|30 - Materializations]] | Explains views, tables, incremental models, ephemeral models, materialized views, and when each fits. |
| [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys|31 - Incremental Models and Unique Keys]] | Core performance pattern for large facts and event streams. |
| [[02 dbt/04 Incremental Processing and Performance/32 Incremental Strategies|32 - Incremental Strategies]] | Covers append, merge, delete+insert, insert overwrite, and adapter-specific behavior. |
| [[02 dbt/04 Incremental Processing and Performance/33 Microbatch Incremental Models|33 - Microbatch Incremental Models]] | Important for large time-series workloads such as trades, prices, risk snapshots, and intraday events. |
| [[02 dbt/04 Incremental Processing and Performance/34 Parallel Microbatch Execution|34 - Parallel Microbatch Execution]] | Speeds up batch windows but needs careful warehouse and dependency design. |
| [[02 dbt/04 Incremental Processing and Performance/35 Snapshots vs Incremental Models vs Dynamic Tables|35 - Snapshots vs Incremental Models vs Dynamic Tables]] | Useful cross-tool decision area for history, freshness, and cost. |
| [[02 dbt/04 Incremental Processing and Performance/36 Model Selection State and Deferral|36 - Model Selection, State, and Deferral]] | Makes CI and selective production builds practical in large projects. |
| [[02 dbt/04 Incremental Processing and Performance/37 Threads Warehouse Sizing and Snowflake Cost|37 - Threads, Warehouse Sizing, and Snowflake Cost]] | Connects dbt concurrency to Snowflake queueing, credit use, and workload isolation. |
| [[02 dbt/04 Incremental Processing and Performance/38 Query Tuning Feedback Loop|38 - Query Tuning Feedback Loop]] | Teaches when to change dbt SQL, model shape, materialization, clustering, or warehouse strategy. |
| [[02 dbt/04 Incremental Processing and Performance/39 Full Refreshes Backfills and Replay|39 - Full Refreshes, Backfills, and Replay]] | Required for corrections, new logic, historical rebuilds, and disaster recovery. |

## 05 Deployment CI CD and Operations

Main hub: [[02 dbt/05 Deployment CI CD and Operations/Deployment CI CD and Operations Overview|Deployment CI CD and Operations Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/05 Deployment CI CD and Operations/40 Git Workflow and Pull Requests|40 - Git Workflow and Pull Requests]] | dbt projects are codebases; review discipline matters. |
| [[02 dbt/05 Deployment CI CD and Operations/41 Dev CI Staging and Prod Environments|41 - Dev, CI, Staging, and Prod Environments]] | Prevents local testing, CI validation, and production execution from stepping on each other. |
| [[02 dbt/05 Deployment CI CD and Operations/42 CI Jobs and Slim CI|42 - CI Jobs and Slim CI]] | Builds only modified resources and their dependents where possible. |
| [[02 dbt/05 Deployment CI CD and Operations/43 Deploy Jobs and Merge Jobs|43 - Deploy Jobs and Merge Jobs]] | Defines how approved code becomes production data. |
| [[02 dbt/05 Deployment CI CD and Operations/44 Scheduling and Orchestration|44 - Scheduling and Orchestration]] | Compares dbt jobs, Snowflake Tasks, Airflow, Dagster, and enterprise schedulers. |
| [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results|45 - Artifacts, Logs, and Run Results]] | Gives operational evidence for debugging, lineage, audit, and incident review. |
| [[02 dbt/05 Deployment CI CD and Operations/46 Package Management and Dependency Governance|46 - Package Management and Dependency Governance]] | Covers version pinning, packages.yml, dependencies.yml, private packages, and supply-chain review. |
| [[02 dbt/05 Deployment CI CD and Operations/47 Secrets Service Accounts and RBAC|47 - Secrets, Service Accounts, and RBAC]] | Central in banking environments where execution identity must be controlled. |
| [[02 dbt/05 Deployment CI CD and Operations/48 Incident Response Rollback and Replay|48 - Incident Response, Rollback, and Replay]] | Turns dbt from "SQL that runs" into an operated production service. |
| [[02 dbt/05 Deployment CI CD and Operations/49 Observability with dbt and Snowflake Metadata|49 - Observability with dbt and Snowflake Metadata]] | Combines dbt artifacts, job history, Snowflake query history, cost data, and freshness signals. |

## 06 Governance Semantic Layer and Mesh

Main hub: [[02 dbt/06 Governance Semantic Layer and Mesh/Governance Semantic Layer and Mesh Overview|Governance Semantic Layer and Mesh Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/06 Governance Semantic Layer and Mesh/50 Groups and Ownership|50 - Groups and Ownership]] | Assigns accountability for related models and domains. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/51 Model Access|51 - Model Access]] | Controls which models are private implementation details and which are stable interfaces. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/52 Model Contracts|52 - Model Contracts]] | Defines expected columns, types, and constraints for important models. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/53 Model Versions and Deprecation|53 - Model Versions and Deprecation]] | Lets teams evolve models without breaking downstream users suddenly. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/54 dbt Mesh and Project Dependencies|54 - dbt Mesh and Project Dependencies]] | Useful for large organizations with multiple domain-owned dbt projects. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/55 Semantic Models and Metrics|55 - Semantic Models and Metrics]] | Defines business metrics closer to governed transformation logic. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/56 Semantic Layer vs BI Metrics vs Snowflake Semantic Views|56 - Semantic Layer vs BI Metrics vs Snowflake Semantic Views]] | Helps decide where metric definitions should live. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/57 Metadata Lineage and Catalog Strategy|57 - Metadata, Lineage, and Catalog Strategy]] | Frames what dbt owns versus Snowflake Horizon, BI catalogs, and enterprise governance tools. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/58 Domain Ownership in Banking|58 - Domain Ownership in Banking]] | Applies data product thinking to risk, finance, operations, trading, compliance, and treasury. |
| [[02 dbt/06 Governance Semantic Layer and Mesh/59 Sensitive Data and Regulatory Boundaries|59 - Sensitive Data and Regulatory Boundaries]] | Connects dbt model design to masking, row access, object tags, PII, MNPI, and audit requirements. |

## 07 Packages Macros and Advanced Reuse

Main hub: [[02 dbt/07 Packages Macros and Advanced Reuse/Packages Macros and Advanced Reuse Overview|Packages Macros and Advanced Reuse Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/07 Packages Macros and Advanced Reuse/60 Package Fundamentals|60 - Package Fundamentals]] | Explains packages as reusable dbt projects containing macros, models, tests, and resources. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/61 packages.yml vs dependencies.yml|61 - packages.yml vs dependencies.yml]] | Distinguishes ordinary package installation from dbt Mesh-style project dependencies. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/62 Must-Have Utility Packages|62 - Must-Have Utility Packages]] | Covers dbt_utils, codegen, audit_helper, and when they save real time. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/63 Data Quality Packages|63 - Data Quality Packages]] | Covers dbt_expectations, elementary, dbt_project_evaluator, and when to avoid test sprawl. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/64 Snowflake and Operations Packages|64 - Snowflake and Operations Packages]] | Covers dbt_snowflake_monitoring, query-tag packages, dbt_external_tables, and Snowflake-specific caveats. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/65 Finance and Bank-Relevant Packages|65 - Finance and Bank-Relevant Packages]] | Evaluates Data Vault packages, constraints, audit packages, observability, metadata testing, and package approval risk. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/66 Package Governance|66 - Package Governance]] | Covers version pinning, Fusion compatibility, package review, transitive dependencies, support boundaries, and regulated-environment approval. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/67 Jinja Fundamentals|67 - Jinja Fundamentals]] | Explains templating, variables, loops, conditionals, whitespace control, and compilation. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/68 Macros as Reusable SQL Functions|68 - Macros as Reusable SQL Functions]] | Teaches how to remove repetition without hiding business logic. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/69 Custom Generic Tests|69 - Custom Generic Tests]] | Shows how macros become reusable data quality checks. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/70 Adapter Dispatch|70 - Adapter Dispatch]] | Explains warehouse-specific behavior behind a common macro interface. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/71 Hooks and Operations|71 - Hooks and Operations]] | Covers on-run-start, on-run-end, grants, audit logging, and dbt run-operation. |
| [[02 dbt/07 Packages Macros and Advanced Reuse/72 Advanced Macro Boundaries|72 - Advanced Macro Boundaries]] | Teaches when abstraction becomes harmful: unreadable compiled SQL, hidden dependencies, and hard-to-debug behavior. |

## 08 dbt on Snowflake and Finance Patterns

Main hub: [[02 dbt/08 dbt on Snowflake and Finance Patterns/dbt on Snowflake and Finance Patterns Overview|dbt on Snowflake and Finance Patterns Overview]]

| Topic | Why it matters |
|---|---|
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/73 dbt on Snowflake Operating Model|73 - dbt on Snowflake Operating Model]] | Connects dbt execution to Snowflake warehouses, schemas, roles, Tasks, and query history. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/74 dbt Projects on Snowflake vs dbt Platform vs Self-Operated dbt|74 - dbt Projects on Snowflake vs dbt Platform vs Self-Operated dbt]] | Clarifies control-plane ownership for Snowflake-centric clients. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/75 Snowflake RBAC for dbt|75 - Snowflake RBAC for dbt]] | Designs developer, CI, deployment, scheduler, and production execution roles. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/76 Database Schema and Warehouse Strategy|76 - Database, Schema, and Warehouse Strategy]] | Separates raw, staging, marts, snapshots, CI schemas, and workload-specific warehouses. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/77 Cost Governance for dbt on Snowflake|77 - Cost Governance for dbt on Snowflake]] | Covers schedules, threads, warehouse size, full refreshes, test cost, and query attribution. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/78 Sensitive Data Controls with dbt and Snowflake|78 - Sensitive Data Controls with dbt and Snowflake]] | Applies masking, row access, tags, object ownership, and approved publication patterns. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/79 Investment Bank Case Study|79 - Investment Bank Case Study]] | Reuses trades, positions, prices, FX, instruments, counterparties, P&L, and risk exposure across the dbt curriculum. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/80 Regulatory Evidence and Auditability|80 - Regulatory Evidence and Auditability]] | Connects dbt tests, artifacts, documentation, exposures, and Snowflake history to control evidence. |
| [[02 dbt/08 dbt on Snowflake and Finance Patterns/81 Fivetran to Snowflake to dbt Flow|81 - Fivetran to Snowflake to dbt Flow]] | Prepares for the later Fivetran section by separating ingestion ownership from transformation ownership. |

## Package Learning Priorities

Treat packages as tools to evaluate, not as things to install by default.

| Priority | Packages | Consultant framing |
|---|---|---|
| Must learn | `dbt_utils`, `codegen`, `audit_helper`, `dbt_project_evaluator`, `dbt_expectations` | Common packages that teach reusable macros, scaffolding, comparison, project hygiene, and richer tests. |
| Strong situational value | `elementary`, `dbt_artifacts`, `dbt_date`, `dbt_external_tables`, query-tag packages, `dbt_snowflake_monitoring`, `dbt_constraints` | Useful for observability, metadata, dates, external table automation, Snowflake attribution, monitoring, and stronger constraints. |
| Finance/bank relevance | `audit_helper`, `elementary`, `dbt_expectations`, `dbt_project_evaluator`, `dbt_constraints`, `dbt_snowflake_monitoring`, `automate_dv`, `datavault4dbt`, metadata-testing packages | Relevant where audit evidence, reconciliation, Data Vault, lineage, controls, and Snowflake cost attribution matter. |
| Use with caution | Domain/vendor/source-specific packages and less maintained community packages | Review maintenance, compatibility, license, security posture, and whether the package hides too much business logic. |

## Cross-Tool Context

- [[00 Home/Snowflake Learning Map]]
- [[01 Snowflake/04 Data Engineering/25 dbt on Snowflake]]
- [[80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview]]
- [[80 Comparisons and Decision Notes/Modern Data Stack Overview]]
- [[03 Fivetran/Fivetran Learning Map]]
- [[04 Docker/Docker Learning Map|Docker Learning Map]]
- [[05 APIs/API Learning Map|API Learning Map]]

## Sources To Revisit

- [dbt Docs: Packages](https://docs.getdbt.com/docs/build/packages)
- [dbt Docs: Jinja and macros](https://docs.getdbt.com/docs/build/jinja-macros)
- [dbt Docs: Project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies)
- [dbt Docs: Incremental models](https://docs.getdbt.com/docs/build/incremental-models)
- [dbt Docs: Microbatch incremental models](https://docs.getdbt.com/docs/build/incremental-microbatch)
- [dbt Docs: Continuous integration](https://docs.getdbt.com/docs/deploy/continuous-integration)
- [dbt Docs: Model governance](https://docs.getdbt.com/docs/mesh/govern/about-model-governance)
- [dbt Docs: Semantic models](https://docs.getdbt.com/docs/build/semantic-models)
- [dbt Hub](https://hub.getdbt.com/)

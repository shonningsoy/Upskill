---
status: hub
platform: dbt
area: dbt on Snowflake and Finance Patterns
tags:
  - dbt
  - dbt-snowflake-finance
  - map
---

# dbt on Snowflake and Finance Patterns Overview

> Applying dbt in a Snowflake-backed investment-bank environment: RBAC, warehouses, cost, sensitive data, controls, and platform boundaries.

## Topics

- [[02 dbt/08 dbt on Snowflake and Finance Patterns/73 dbt on Snowflake Operating Model|73 - dbt on Snowflake Operating Model]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/74 dbt Projects on Snowflake vs dbt Platform vs Self-Operated dbt|74 - dbt Projects on Snowflake vs dbt Platform vs Self-Operated dbt]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/75 Snowflake RBAC for dbt|75 - Snowflake RBAC for dbt]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/76 Database Schema and Warehouse Strategy|76 - Database, Schema, and Warehouse Strategy]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/77 Cost Governance for dbt on Snowflake|77 - Cost Governance for dbt on Snowflake]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/78 Sensitive Data Controls with dbt and Snowflake|78 - Sensitive Data Controls with dbt and Snowflake]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/79 Investment Bank Case Study|79 - Investment Bank Case Study]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/80 Regulatory Evidence and Auditability|80 - Regulatory Evidence and Auditability]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/81 Fivetran to Snowflake to dbt Flow|81 - Fivetran to Snowflake to dbt Flow]]

## Topic Summaries

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/73 dbt on Snowflake Operating Model|73 - dbt on Snowflake Operating Model]]

Connects dbt execution to Snowflake warehouses, schemas, roles, Tasks, and query history.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/74 dbt Projects on Snowflake vs dbt Platform vs Self-Operated dbt|74 - dbt Projects on Snowflake vs dbt Platform vs Self-Operated dbt]]

Clarifies control-plane ownership for Snowflake-centric clients.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/75 Snowflake RBAC for dbt|75 - Snowflake RBAC for dbt]]

Designs developer, CI, deployment, scheduler, and production execution roles.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/76 Database Schema and Warehouse Strategy|76 - Database, Schema, and Warehouse Strategy]]

Separates raw, staging, marts, snapshots, CI schemas, and workload-specific warehouses.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/77 Cost Governance for dbt on Snowflake|77 - Cost Governance for dbt on Snowflake]]

Covers schedules, threads, warehouse size, full refreshes, test cost, and query attribution.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/78 Sensitive Data Controls with dbt and Snowflake|78 - Sensitive Data Controls with dbt and Snowflake]]

Applies masking, row access, tags, object ownership, and approved publication patterns.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/79 Investment Bank Case Study|79 - Investment Bank Case Study]]

Reuses trades, positions, prices, FX, instruments, counterparties, P&L, and risk exposure across the dbt curriculum.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/80 Regulatory Evidence and Auditability|80 - Regulatory Evidence and Auditability]]

Connects dbt tests, artifacts, documentation, exposures, and Snowflake history to control evidence.

### [[02 dbt/08 dbt on Snowflake and Finance Patterns/81 Fivetran to Snowflake to dbt Flow|81 - Fivetran to Snowflake to dbt Flow]]

Prepares for the later Fivetran section by separating ingestion ownership from transformation ownership.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/Packages Macros and Advanced Reuse Overview|Previous: Packages Macros and Advanced Reuse]]
- [[01 Snowflake/04 Data Engineering/25 dbt on Snowflake|dbt on Snowflake]]
- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses|Snowflake Virtual Warehouses]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges|Snowflake RBAC Roles and Privileges]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model|Snowflake Credit Consumption Model]]
- [[01 Snowflake/08 Enterprise Snowflake in Production/54 Horizon Catalog, Lineage, and Access History|Snowflake Horizon Catalog, Lineage, and Access History]]

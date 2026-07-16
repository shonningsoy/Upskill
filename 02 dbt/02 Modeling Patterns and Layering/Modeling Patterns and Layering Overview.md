---
status: hub
platform: dbt
area: Modeling Patterns and Layering
tags:
  - dbt
  - dbt-modeling
  - map
---

# Modeling Patterns and Layering Overview

> How dbt projects shape raw source data into reliable analytical models, marts, and finance-ready data products.

## Topics

- [[02 dbt/02 Modeling Patterns and Layering/11 Staging Models|11 - Staging Models]]
- [[02 dbt/02 Modeling Patterns and Layering/12 Intermediate Models|12 - Intermediate Models]]
- [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products|13 - Marts and Data Products]]
- [[02 dbt/02 Modeling Patterns and Layering/14 Dimensional Modeling with dbt|14 - Dimensional Modeling with dbt]]
- [[02 dbt/02 Modeling Patterns and Layering/15 Finance Modeling Patterns|15 - Finance Modeling Patterns]]
- [[02 dbt/02 Modeling Patterns and Layering/16 Naming Conventions and Folder Design|16 - Naming Conventions and Folder Design]]
- [[02 dbt/02 Modeling Patterns and Layering/17 Refactoring Legacy SQL into dbt|17 - Refactoring Legacy SQL into dbt]]
- [[02 dbt/02 Modeling Patterns and Layering/18 Multi-source Conformed Models|18 - Multi-source Conformed Models]]
- [[02 dbt/02 Modeling Patterns and Layering/19 Late-arriving Data Corrections and Restatements|19 - Late-arriving Data, Corrections, and Restatements]]
- [[02 dbt/02 Modeling Patterns and Layering/20 Reconciliation Models|20 - Reconciliation Models]]

## Topic Summaries

### [[02 dbt/02 Modeling Patterns and Layering/11 Staging Models|11 - Staging Models]]

Standardizes raw source data into clean, typed, renamed, lightly transformed building blocks.

### [[02 dbt/02 Modeling Patterns and Layering/12 Intermediate Models|12 - Intermediate Models]]

Keeps complex business logic readable without exposing every step as a consumer-facing asset.

### [[02 dbt/02 Modeling Patterns and Layering/13 Marts and Data Products|13 - Marts and Data Products]]

Frames dbt output as trusted analytical products for BI, risk, finance, and regulatory use.

### [[02 dbt/02 Modeling Patterns and Layering/14 Dimensional Modeling with dbt|14 - Dimensional Modeling with dbt]]

Gives structure for facts, dimensions, grain, keys, and conformed entities.

### [[02 dbt/02 Modeling Patterns and Layering/15 Finance Modeling Patterns|15 - Finance Modeling Patterns]]

Covers trades, orders, executions, positions, instruments, counterparties, accounts, FX, prices, P&L, and risk measures.

### [[02 dbt/02 Modeling Patterns and Layering/16 Naming Conventions and Folder Design|16 - Naming Conventions and Folder Design]]

Makes large projects navigable and reduces onboarding friction.

### [[02 dbt/02 Modeling Patterns and Layering/17 Refactoring Legacy SQL into dbt|17 - Refactoring Legacy SQL into dbt]]

Highly relevant in consulting, where existing SQL estates rarely start clean.

### [[02 dbt/02 Modeling Patterns and Layering/18 Multi-source Conformed Models|18 - Multi-source Conformed Models]]

Handles the reality of combining trading, risk, finance, reference, and market-data sources.

### [[02 dbt/02 Modeling Patterns and Layering/19 Late-arriving Data Corrections and Restatements|19 - Late-arriving Data, Corrections, and Restatements]]

Essential in banks where adjustments and backdated corrections are normal.

### [[02 dbt/02 Modeling Patterns and Layering/20 Reconciliation Models|20 - Reconciliation Models]]

Provides evidence that trade, position, cash, P&L, and regulatory numbers tie out.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/01 Core Concepts and Project Structure/Core Concepts and Project Structure Overview|Previous: Core Concepts and Project Structure]]
- [[02 dbt/03 Testing Documentation and Data Quality/Testing Documentation and Data Quality Overview|Next: Testing Documentation and Data Quality]]

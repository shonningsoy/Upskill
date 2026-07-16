---
status: hub
platform: dbt
area: Testing Documentation and Data Quality
tags:
  - dbt
  - dbt-quality-docs
  - map
---

# Testing Documentation and Data Quality Overview

> The trust layer around dbt: tests, documentation, source freshness, exposures, contracts, and audit-oriented quality controls.

## Topics

- [[02 dbt/03 Testing Documentation and Data Quality/21 Generic Singular and Custom Data Tests|21 - Generic, Singular, and Custom Data Tests]]
- [[02 dbt/03 Testing Documentation and Data Quality/22 Unit Tests for SQL Logic|22 - Unit Tests for SQL Logic]]
- [[02 dbt/03 Testing Documentation and Data Quality/23 Source Freshness and SLA Monitoring|23 - Source Freshness and SLA Monitoring]]
- [[02 dbt/03 Testing Documentation and Data Quality/24 Documentation Blocks and Catalog|24 - Documentation Blocks and Catalog]]
- [[02 dbt/03 Testing Documentation and Data Quality/25 Exposures|25 - Exposures]]
- [[02 dbt/03 Testing Documentation and Data Quality/26 Model Contracts and Constraints|26 - Model Contracts and Constraints]]
- [[02 dbt/03 Testing Documentation and Data Quality/27 Test Severity and Failure Handling|27 - Test Severity and Failure Handling]]
- [[02 dbt/03 Testing Documentation and Data Quality/28 Audit and Migration Validation|28 - Audit and Migration Validation]]
- [[02 dbt/03 Testing Documentation and Data Quality/29 Data Quality Strategy in Regulated Environments|29 - Data Quality Strategy in Regulated Environments]]

## Topic Summaries

### [[02 dbt/03 Testing Documentation and Data Quality/21 Generic Singular and Custom Data Tests|21 - Generic, Singular, and Custom Data Tests]]

Core dbt quality mechanism and the basis for consultant discussions about trust.

### [[02 dbt/03 Testing Documentation and Data Quality/22 Unit Tests for SQL Logic|22 - Unit Tests for SQL Logic]]

Validates transformation logic on controlled inputs before expensive production builds.

### [[02 dbt/03 Testing Documentation and Data Quality/23 Source Freshness and SLA Monitoring|23 - Source Freshness and SLA Monitoring]]

Separates "the model ran" from "the upstream data was actually current."

### [[02 dbt/03 Testing Documentation and Data Quality/24 Documentation Blocks and Catalog|24 - Documentation Blocks and Catalog]]

Makes model and column definitions reusable and reviewable.

### [[02 dbt/03 Testing Documentation and Data Quality/25 Exposures|25 - Exposures]]

Links dbt assets to dashboards, reports, ML jobs, regulatory outputs, and owners.

### [[02 dbt/03 Testing Documentation and Data Quality/26 Model Contracts and Constraints|26 - Model Contracts and Constraints]]

Creates stronger producer-consumer expectations for important interfaces.

### [[02 dbt/03 Testing Documentation and Data Quality/27 Test Severity and Failure Handling|27 - Test Severity and Failure Handling]]

Determines whether failed tests should warn, block publication, alert, or trigger incident handling.

### [[02 dbt/03 Testing Documentation and Data Quality/28 Audit and Migration Validation|28 - Audit and Migration Validation]]

Uses row counts, checksums, and diff logic to prove refactors or migrations are safe.

### [[02 dbt/03 Testing Documentation and Data Quality/29 Data Quality Strategy in Regulated Environments|29 - Data Quality Strategy in Regulated Environments]]

Connects tests to control evidence, ownership, sign-off, and operational accountability.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/02 Modeling Patterns and Layering/Modeling Patterns and Layering Overview|Previous: Modeling Patterns and Layering]]
- [[02 dbt/04 Incremental Processing and Performance/Incremental Processing and Performance Overview|Next: Incremental Processing and Performance]]

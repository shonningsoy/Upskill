---
status: hub
platform: dbt
area: Governance Semantic Layer and Mesh
tags:
  - dbt
  - dbt-governance-mesh
  - map
---

# Governance Semantic Layer and Mesh Overview

> How dbt supports ownership, stable interfaces, semantic definitions, cross-project dependencies, and governed domains.

## Topics

- [[02 dbt/06 Governance Semantic Layer and Mesh/50 Groups and Ownership|50 - Groups and Ownership]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/51 Model Access|51 - Model Access]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/52 Model Contracts|52 - Model Contracts]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/53 Model Versions and Deprecation|53 - Model Versions and Deprecation]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/54 dbt Mesh and Project Dependencies|54 - dbt Mesh and Project Dependencies]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/55 Semantic Models and Metrics|55 - Semantic Models and Metrics]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/56 Semantic Layer vs BI Metrics vs Snowflake Semantic Views|56 - Semantic Layer vs BI Metrics vs Snowflake Semantic Views]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/57 Metadata Lineage and Catalog Strategy|57 - Metadata, Lineage, and Catalog Strategy]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/58 Domain Ownership in Banking|58 - Domain Ownership in Banking]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/59 Sensitive Data and Regulatory Boundaries|59 - Sensitive Data and Regulatory Boundaries]]

## Topic Summaries

### [[02 dbt/06 Governance Semantic Layer and Mesh/50 Groups and Ownership|50 - Groups and Ownership]]

Assigns accountability for related models and domains.

### [[02 dbt/06 Governance Semantic Layer and Mesh/51 Model Access|51 - Model Access]]

Controls which models are private implementation details and which are stable interfaces.

### [[02 dbt/06 Governance Semantic Layer and Mesh/52 Model Contracts|52 - Model Contracts]]

Defines expected columns, types, and constraints for important models.

### [[02 dbt/06 Governance Semantic Layer and Mesh/53 Model Versions and Deprecation|53 - Model Versions and Deprecation]]

Lets teams evolve models without breaking downstream users suddenly.

### [[02 dbt/06 Governance Semantic Layer and Mesh/54 dbt Mesh and Project Dependencies|54 - dbt Mesh and Project Dependencies]]

Useful for large organizations with multiple domain-owned dbt projects.

### [[02 dbt/06 Governance Semantic Layer and Mesh/55 Semantic Models and Metrics|55 - Semantic Models and Metrics]]

Defines business metrics closer to governed transformation logic.

### [[02 dbt/06 Governance Semantic Layer and Mesh/56 Semantic Layer vs BI Metrics vs Snowflake Semantic Views|56 - Semantic Layer vs BI Metrics vs Snowflake Semantic Views]]

Helps decide where metric definitions should live.

### [[02 dbt/06 Governance Semantic Layer and Mesh/57 Metadata Lineage and Catalog Strategy|57 - Metadata, Lineage, and Catalog Strategy]]

Frames what dbt owns versus Snowflake Horizon, BI catalogs, and enterprise governance tools.

### [[02 dbt/06 Governance Semantic Layer and Mesh/58 Domain Ownership in Banking|58 - Domain Ownership in Banking]]

Applies data product thinking to risk, finance, operations, trading, compliance, and treasury.

### [[02 dbt/06 Governance Semantic Layer and Mesh/59 Sensitive Data and Regulatory Boundaries|59 - Sensitive Data and Regulatory Boundaries]]

Connects dbt model design to masking, row access, object tags, PII, MNPI, and audit requirements.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/05 Deployment CI CD and Operations/Deployment CI CD and Operations Overview|Previous: Deployment CI CD and Operations]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/Packages Macros and Advanced Reuse Overview|Next: Packages Macros and Advanced Reuse]]

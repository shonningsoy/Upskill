# Fivetran Learning Map

> [!abstract] Learning outcome
> Explain how Fivetran moves and maintains data, evaluate connector fit, design a governed Snowflake implementation, and recommend an operating and cost model to a client.

Use this as the main entry point for Fivetran. The graph follows the same pattern as Snowflake and dbt:

`Fivetran Learning Map -> chapter overview -> individual topic notes`

The curriculum is intentionally smaller than the Snowflake and dbt curricula. Fivetran is primarily a managed data-movement platform, so the focus is on data behavior, configuration, operational ownership, controls, and recommendation trade-offs rather than extensive code.

## Learning Path

```mermaid
flowchart LR
    A["1<br/>Build the platform mental model"] --> B["2<br/>Understand connectors and syncs"]
    B --> C["3<br/>Interpret destination data"]
    C --> D["4<br/>Apply it to Snowflake and dbt"]
    D --> E["5<br/>Govern and operate it"]
    E --> F["6<br/>Evaluate cost and recommend"]

    classDef foundation fill:#E8F0FE,stroke:#4C6EF5,color:#172B4D
    classDef movement fill:#E6FCF5,stroke:#0CA678,color:#163A2D
    classDef control fill:#FFF4E6,stroke:#F08C00,color:#4A2A00
    classDef decision fill:#F3F0FF,stroke:#7950F2,color:#2B1B54

    class A foundation
    class B,C movement
    class D,E control
    class F decision
```

## Suggested Path

1. Foundation: understand what Fivetran owns, how the platform is organized, and when managed ELT fits.
2. Connectors and syncs: understand how source type changes extraction behavior, latency, recovery, and risk.
3. Destination data: learn row identity, delete and history semantics, schema drift, and reconciliation boundaries.
4. Snowflake and dbt: apply the concepts to the target stack and separate ingestion from transformation ownership.
5. Production controls: learn security, connectivity, monitoring, incident response, and automation.
6. Commercial judgment: learn MAR, total cost, and how to frame a client recommendation.

## Chapter Hubs

- [[03 Fivetran/01 Foundations and Platform Mental Model/Foundations and Platform Mental Model Overview|01 - Foundations and Platform Mental Model]]
- [[03 Fivetran/02 Connectors and Sync Behavior/Connectors and Sync Behavior Overview|02 - Connectors and Sync Behavior]]
- [[03 Fivetran/03 Destination Data History and Schema Change/Destination Data History and Schema Change Overview|03 - Destination Data, History, and Schema Change]]
- [[03 Fivetran/04 Fivetran with Snowflake and dbt/Fivetran with Snowflake and dbt Overview|04 - Fivetran with Snowflake and dbt]]
- [[03 Fivetran/05 Security Governance and Production Operations/Security Governance and Production Operations Overview|05 - Security, Governance, and Production Operations]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/Cost and Consultant Decision-Making Overview|06 - Cost and Consultant Decision-Making]]

## 01 Foundations and Platform Mental Model

| Topic | Why it matters |
|---|---|
| [[03 Fivetran/01 Foundations and Platform Mental Model/01 What Fivetran Is and Is Not|01 - What Fivetran Is and Is Not]] | Positions Fivetran correctly as managed data movement rather than storage, business modeling, or universal orchestration. |
| [[03 Fivetran/01 Foundations and Platform Mental Model/02 Platform Anatomy and End-to-End Data Flow|02 - Platform Anatomy and End-to-End Data Flow]] | Establishes the resource hierarchy and follows data from source to destination. |
| [[03 Fivetran/01 Foundations and Platform Mental Model/03 The Connection Lifecycle|03 - The Connection Lifecycle]] | Connects setup, initial sync, steady-state operation, recovery, and retirement. |
| [[03 Fivetran/01 Foundations and Platform Mental Model/04 Connections Transformations and Activations|04 - Connections, Transformations, and Activations]] | Separates ingestion, in-warehouse transformation, and reverse ETL responsibilities. |
| [[03 Fivetran/01 Foundations and Platform Mental Model/05 When to Recommend Fivetran|05 - When to Recommend Fivetran]] | Frames managed standardization against cost, control, latency, and unsupported-source requirements. |

## 02 Connectors and Sync Behavior

| Topic | Why it matters |
|---|---|
| [[03 Fivetran/02 Connectors and Sync Behavior/06 Connector Types Coverage and Maturity|06 - Connector Types, Coverage, and Maturity]] | Teaches how source category, feature coverage, and connector maturity affect risk. |
| [[03 Fivetran/02 Connectors and Sync Behavior/07 Application and API Connectors|07 - Application and API Connectors]] | Covers API quotas, provider-defined schemas, accessible history, and source-specific behavior. |
| [[03 Fivetran/02 Connectors and Sync Behavior/08 Database Connectors CDC and High-Volume Agent|08 - Database Connectors, CDC, and High-Volume Agent]] | Explains log-based change capture, permissions, retention, source impact, and high-volume patterns. |
| [[03 Fivetran/02 Connectors and Sync Behavior/09 File Event and Custom Connector Patterns|09 - File, Event, and Custom Connector Patterns]] | Introduces non-database ingestion and when custom connector ownership becomes necessary. |
| [[03 Fivetran/02 Connectors and Sync Behavior/10 Initial Incremental Re-import and Re-sync Strategies|10 - Initial, Incremental, Re-import, and Re-sync Strategies]] | Distinguishes the main ways data is first loaded, maintained, and reconstructed. |
| [[03 Fivetran/02 Connectors and Sync Behavior/11 Scheduling Latency Checkpoints and Recovery|11 - Scheduling, Latency, Checkpoints, and Recovery]] | Separates configured frequency from actual freshness and explains recoverability at consultant depth. |

## 03 Destination Data, History, and Schema Change

| Topic | Why it matters |
|---|---|
| [[03 Fivetran/03 Destination Data History and Schema Change/12 Soft Delete Mode vs History Mode|12 - Soft Delete Mode vs History Mode]] | Compares current-state replication with preserving row versions. |
| [[03 Fivetran/03 Destination Data History and Schema Change/13 Keys Deletes and Fivetran System Columns|13 - Keys, Deletes, and Fivetran System Columns]] | Shows how row identity and Fivetran metadata affect correctness, querying, and cost. |
| [[03 Fivetran/03 Destination Data History and Schema Change/14 Destination Schemas Naming and Data Type Mapping|14 - Destination Schemas, Naming, and Data Type Mapping]] | Explains how source structures appear and change shape in the destination. |
| [[03 Fivetran/03 Destination Data History and Schema Change/15 Schema Change Handling and Data Selection Controls|15 - Schema Change Handling and Data Selection Controls]] | Covers schema drift policies, table and column scope, filtering, blocking, and hashing. |
| [[03 Fivetran/03 Destination Data History and Schema Change/16 Data Contracts Completeness and Reconciliation|16 - Data Contracts, Completeness, and Reconciliation]] | Defines what must be validated downstream even when a sync reports success. |

## 04 Fivetran with Snowflake and dbt

| Topic | Why it matters |
|---|---|
| [[03 Fivetran/04 Fivetran with Snowflake and dbt/17 Setting Up Snowflake as a Destination|17 - Setting Up Snowflake as a Destination]] | Connects Fivetran setup to Snowflake identity, privileges, authentication, and networking. |
| [[03 Fivetran/04 Fivetran with Snowflake and dbt/18 Snowflake Database Schema Warehouse and Cost Design|18 - Snowflake Database, Schema, Warehouse, and Cost Design]] | Designs raw landing zones, workload isolation, auto-suspend, and storage trade-offs. |
| [[03 Fivetran/04 Fivetran with Snowflake and dbt/19 Fivetran to Snowflake to dbt Ownership Boundaries|19 - Fivetran to Snowflake to dbt Ownership Boundaries]] | Clarifies which tool owns ingestion, storage, transformation, testing, and publication. |
| [[03 Fivetran/04 Fivetran with Snowflake and dbt/20 Transformation and Orchestration Options|20 - Transformation and Orchestration Options]] | Compares Fivetran scheduling and transformation options with external dbt orchestration. |
| [[03 Fivetran/04 Fivetran with Snowflake and dbt/21 Finance and Banking End-to-End Case Study|21 - Finance and Banking End-to-End Case Study]] | Applies connector, control, reconciliation, cost, and ownership decisions to a realistic regulated environment. |

## 05 Security, Governance, and Production Operations

| Topic | Why it matters |
|---|---|
| [[03 Fivetran/05 Security Governance and Production Operations/22 Security Architecture and Shared Responsibility|22 - Security Architecture and Shared Responsibility]] | Separates platform controls from client-owned source, destination, identity, and monitoring controls. |
| [[03 Fivetran/05 Security Governance and Production Operations/23 SaaS Hybrid and Private Connectivity Patterns|23 - SaaS, Hybrid, and Private Connectivity Patterns]] | Matches deployment and network design to compliance and operational requirements. |
| [[03 Fivetran/05 Security Governance and Production Operations/24 Credentials RBAC SSO SCIM and Service Access|24 - Credentials, RBAC, SSO, SCIM, and Service Access]] | Covers human and service identity, least privilege, provisioning, rotation, and revocation. |
| [[03 Fivetran/05 Security Governance and Production Operations/25 Privacy Compliance Audit and Vendor Risk|25 - Privacy, Compliance, Audit, and Vendor Risk]] | Frames data residency, sensitive-data exposure, evidence, contracts, and third-party risk. |
| [[03 Fivetran/05 Security Governance and Production Operations/26 Monitoring Logging Freshness and the Platform Connector|26 - Monitoring, Logging, Freshness, and the Platform Connector]] | Turns dashboard and metadata signals into production observability and audit evidence. |
| [[03 Fivetran/05 Security Governance and Production Operations/27 Incident Response Re-syncs and Recovery Runbooks|27 - Incident Response, Re-syncs, and Recovery Runbooks]] | Guides diagnosis and selects the least disruptive recovery action. |
| [[03 Fivetran/05 Security Governance and Production Operations/28 REST API Terraform and Configuration Automation|28 - REST API, Terraform, and Configuration Automation]] | Introduces repeatable provisioning, configuration governance, and automation boundaries. |

## 06 Cost and Consultant Decision-Making

| Topic | Why it matters |
|---|---|
| [[03 Fivetran/06 Cost and Consultant Decision-Making/29 Monthly Active Rows|29 - Monthly Active Rows]] | Establishes the core pricing mental model around distinct changed rows. |
| [[03 Fivetran/06 Cost and Consultant Decision-Making/30 Forecasting Cost Drivers and Usage Optimization|30 - Forecasting, Cost Drivers, and Usage Optimization]] | Identifies likely consumption and practical ways to control it. |
| [[03 Fivetran/06 Cost and Consultant Decision-Making/31 Destination Cost and End-to-End Total Cost of Ownership|31 - Destination Cost and End-to-End Total Cost of Ownership]] | Expands the commercial view beyond the Fivetran invoice to Snowflake and internal operating cost. |
| [[03 Fivetran/06 Cost and Consultant Decision-Making/32 Fivetran Recommendation and Enterprise Adoption Framework|32 - Fivetran Recommendation and Enterprise Adoption Framework]] | Brings technical fit, control, cost, pilot design, ownership, and rollout into one recommendation framework. |

## How To Learn Each Topic

1. Start with the mental model and the client problem it addresses.
2. Work through one representative source-to-destination example.
3. Discuss limitations, controls, cost, and operational ownership.
4. Test the recommendation with a short client scenario.
5. Update the seed note only when the discussion has produced durable understanding.

## Cross-Tool Context

- [[00 Home/Snowflake Learning Map|Snowflake Learning Map]]
- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[04 Docker/Docker Learning Map|Docker Learning Map]]
- [[01 Snowflake/04 Data Engineering/25 dbt on Snowflake|dbt on Snowflake]]
- [[02 dbt/08 dbt on Snowflake and Finance Patterns/81 Fivetran to Snowflake to dbt Flow|Fivetran to Snowflake to dbt Flow]]
- [[80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview|Comparisons and Decision Notes]]
- [[80 Comparisons and Decision Notes/Modern Data Stack Overview|Modern Data Stack Overview]]

## Sources To Revisit

- [Fivetran Documentation](https://fivetran.com/docs)
- [Fivetran Core Concepts](https://fivetran.com/docs/core-concepts)
- [Fivetran Connectors](https://fivetran.com/docs/connectors)
- [Fivetran Destinations](https://fivetran.com/docs/destinations)
- [Fivetran Pricing](https://fivetran.com/docs/getting-started/pricing)

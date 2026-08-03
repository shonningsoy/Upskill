# Comparisons and Decision Notes Overview

> Consultant reasoning layer for turning topic knowledge into recommendations.

## How To Use This Area

Use the topic notes to learn what each feature is. Use this area to practice when to recommend features, how to compare trade-offs, and how to reason through client situations.

Keep links curated. A comparison or scenario should usually link to the 3-6 topic notes that genuinely matter for the decision.

Organize this area by note type first, then domain:

```text
Comparisons/Snowflake, Comparisons/dbt, Comparisons/Fivetran, Comparisons/Cross-Tool
Decision Notes/Snowflake, Decision Notes/dbt, Decision Notes/Fivetran, Decision Notes/Cross-Tool
Client Scenarios/Snowflake, Client Scenarios/dbt, Client Scenarios/Fivetran, Client Scenarios/Cross-Tool
```

## Comparisons

### Snowflake

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Virtual Warehouse Size vs Multi-cluster]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Resource Monitors vs Budgets]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Account Usage Views vs Information Schema]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Bigger Warehouse vs Clustering]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Search Optimization Service vs Clustering]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Materialized Views vs Dynamic Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Streams and Tasks vs Dynamic Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Time Travel vs Fail-safe]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Permanent vs Transient vs Temporary Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Direct Share vs Reader Account]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Direct Share vs Marketplace Listing]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Native App vs Streamlit vs Direct Share]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - QAS vs Warehouse Upsizing]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Secondary Roles vs Composite Roles]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - RAP on Base Table vs Views with Separate RAPs]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Manual vs Auto Classification]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Network Policy vs Private Connectivity]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Default Encryption vs Tri-Secret Secure]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - External Tables vs Iceberg Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Cortex Analyst vs Cortex AI Functions]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Cortex Search vs Cortex Analyst]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - AI_EXTRACT vs Cortex Search]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - RAG vs Fine-tuning]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - On-demand Cortex Inference vs Provisioned Throughput]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Warehouse Inference vs SPCS Model Serving]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Snowflake CLI vs Terraform Provider]]

### dbt

- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Snapshots vs Incremental Models]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Full Refresh vs Backfill vs Replay]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Full CI vs Slim CI]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - CI Jobs vs Deploy Jobs]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Seeds vs Managed Reference Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Star Schema vs Wide Marts]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Freshness vs Completeness vs Validity vs Reconciliation]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Model Contracts vs Data Tests vs Warehouse Constraints]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - dbt Packages vs Project Dependencies]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - dbt Governance Controls vs Snowflake Security Controls]]
- [[80 Comparisons and Decision Notes/Comparisons/dbt/Comparison - Hooks vs Models Tests and Operations]]

### Fivetran

- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - Fivetran vs Custom Ingestion]]
- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - Soft Delete Mode vs History Mode]]
- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - SaaS vs Hybrid Deployment]]

### Cross-Tool

- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Time Travel vs Modeled Historical Data]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Conformed Analytical Models vs Master Data Management]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - dbt Projects on Snowflake vs dbt Platform]]
- [[80 Comparisons and Decision Notes/Comparisons/Cross-Tool/Comparison - Stored Procedures vs Declarative Transformations]]

## Decision Notes

### Snowflake

- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Diagnosing Slow Snowflake Queries]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Warehouse Strategy by Workload Type]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing Table Retention by Data Criticality]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake Sharing Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Row-Level Data Isolation Strategy]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake Ingestion Method]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake Notification Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake AI and ML Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Diagnosing Snowflake Spend Increases]]

### dbt

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Environment and Credential Strategy]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Production Deployment Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Scheduling and Orchestration Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Designing dbt Observability and Incident Evidence]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Versioning and Deprecating a dbt Model]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Materialization and Refresh Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Incremental Strategy on Snowflake]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Modeling Layer]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing the Right dbt Quality Control]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a Legacy SQL-to-dbt Migration Strategy]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a Data History and Restatement Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing Where Governed Metrics Should Live]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a Catalog and Lineage Authority]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Approving a dbt Package for Production]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - When to Abstract dbt SQL into Macros]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Designing dbt Roles Schemas and Warehouses on Snowflake]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Controlling and Attributing dbt Cost on Snowflake]]

### Fivetran

- [[80 Comparisons and Decision Notes/Decision Notes/Fivetran/Decisions - Evaluating Fivetran Connector Fit]]
- [[80 Comparisons and Decision Notes/Decision Notes/Fivetran/Decisions - Designing a Fivetran Production Operating Model]]
- [[80 Comparisons and Decision Notes/Decision Notes/Fivetran/Decisions - Estimating and Controlling Fivetran Cost]]

### Cross-Tool

- [[80 Comparisons and Decision Notes/Decision Notes/Cross-Tool/Decisions - Choosing a Snowflake DevOps and Deployment Pattern]]

## Client Scenarios

### Snowflake

- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Dashboards Are Slow During Business Hours]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - One Shared Warehouse Is Causing Conflicts]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Query Scans Too Much Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Daily Load Overwrote Good Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Snowflake Costs Spiked After Retention Change]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Partner Needs Access to Live Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Customer Without Snowflake Needs Data Access]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - New Regulatory Team Needs Cross-Domain Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Client Needs to Revoke Vendor Key Access]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Duplicate Trade Event Arrives Repeatedly]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Trade Batch Must Be Validated Before Publication]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Business Users Want Self-Service Analytics but Metrics Are Inconsistent]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - A Churn Model Is Stuck in a Notebook]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Snowflake Spend Increased but Warehouses Look Normal]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Bank Wants to Distribute a Governed Snowflake App Across Accounts]]

### dbt

- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Dashboard Published From Stale Source Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Bank Needs to Reconstruct Customer Risk Rating at Report Time]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Static Mapping CSV Became a Production Control Risk]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Analyst Accidentally Ran dbt Against Production]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Team Cannot Tell Which Dashboards a Model Change Will Break]]

### Fivetran

- No Fivetran client scenarios yet.

### Cross-Tool

- [[80 Comparisons and Decision Notes/Client Scenarios/Cross-Tool/Scenario - Manual Snowflake Changes Keep Breaking Dev Test Prod Consistency]]

## Related Learning Maps

- [[00 Home/Snowflake Learning Map]]
- [[02 dbt/dbt Learning Map]]
- [[03 Fivetran/Fivetran Learning Map]]
- [[80 Comparisons and Decision Notes/Modern Data Stack Overview]]

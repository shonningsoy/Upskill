---
tags:
  - note-comparison
---

# Comparison - SaaS vs Hybrid Deployment

## Short Answer

Use **SaaS Deployment** when Fivetran-managed processing and the approved network path satisfy the client's data-boundary requirements.

Use **Hybrid Deployment** when supported workloads must process data inside the client's environment and the client can operate the local agent infrastructure.

## Comparison Table

| Dimension | SaaS Deployment | Hybrid Deployment |
|---|---|---|
| Data processing | Fivetran-managed cloud | Client-controlled environment |
| Control plane | Fivetran SaaS | Fivetran SaaS still configures and orchestrates movement |
| Client operations | Lower | Higher: agent capacity, upgrades, networking, monitoring, and recovery |
| Connector support | Broadest default | Must be verified against the current support matrix |
| Plan considerations | Available broadly, with some network features gated | Enterprise or Business Critical requirement under current plans |
| Best fit | Standard managed analytical ingestion | Stronger local-processing or compliance boundary |
| Main risk | Assuming encryption alone satisfies private-routing or residency policy | Assuming local processing removes all SaaS metadata and operational dependencies |

## Decision Rules

- Choose the processing boundary first, then choose direct, tunnel, proxy, VPN, or private endpoint connectivity.
- Trace actual source, staging, destination, metadata, and support paths; do not rely on product labels alone.
- Verify connector, destination, region, plan, and failover support for the exact design.
- Use Hybrid only when the control benefit justifies the client's new operational responsibilities.

## Related Learning Topics

- [[03 Fivetran/05 Security Governance and Production Operations/22 Security Architecture and Shared Responsibility]]
- [[03 Fivetran/05 Security Governance and Production Operations/23 SaaS Hybrid and Private Connectivity Patterns]]
- [[03 Fivetran/05 Security Governance and Production Operations/25 Privacy Compliance Audit and Vendor Risk]]
- [[03 Fivetran/04 Fivetran with Snowflake and dbt/17 Setting Up Snowflake as a Destination]]

## Related Scenarios

- No dedicated scenario note yet.

## Sources To Revisit

- [Fivetran Deployment Models](https://fivetran.com/docs/core-concepts/deployment-models)
- [Fivetran SaaS Deployment](https://fivetran.com/docs/core-concepts/deployment-models/saas-deployment)
- [Fivetran Hybrid Deployment](https://fivetran.com/docs/core-concepts/deployment-models/hybrid-deployment)

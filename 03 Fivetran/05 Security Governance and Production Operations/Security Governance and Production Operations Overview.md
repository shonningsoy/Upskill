---
status: hub
platform: Fivetran
area: Security Governance and Production Operations
tags:
  - fivetran
  - fivetran-security-operations
  - map
---

# Security, Governance, and Production Operations Overview

> Turn managed data movement into a governed production service with explicit identities, network boundaries, monitoring, recovery, and automation.

> [!abstract] Chapter outcome
> You should be able to assess deployment and access controls, define operational evidence, respond to incidents, and identify where automation improves consistency.

## Topics

- [[03 Fivetran/05 Security Governance and Production Operations/22 Security Architecture and Shared Responsibility|22 - Security Architecture and Shared Responsibility]]
- [[03 Fivetran/05 Security Governance and Production Operations/23 SaaS Hybrid and Private Connectivity Patterns|23 - SaaS, Hybrid, and Private Connectivity Patterns]]
- [[03 Fivetran/05 Security Governance and Production Operations/24 Credentials RBAC SSO SCIM and Service Access|24 - Credentials, RBAC, SSO, SCIM, and Service Access]]
- [[03 Fivetran/05 Security Governance and Production Operations/25 Privacy Compliance Audit and Vendor Risk|25 - Privacy, Compliance, Audit, and Vendor Risk]]
- [[03 Fivetran/05 Security Governance and Production Operations/26 Monitoring Logging Freshness and the Platform Connector|26 - Monitoring, Logging, Freshness, and the Platform Connector]]
- [[03 Fivetran/05 Security Governance and Production Operations/27 Incident Response Re-syncs and Recovery Runbooks|27 - Incident Response, Re-syncs, and Recovery Runbooks]]
- [[03 Fivetran/05 Security Governance and Production Operations/28 REST API Terraform and Configuration Automation|28 - REST API, Terraform, and Configuration Automation]]

## Chapter Map

```mermaid
flowchart LR
    A["Map responsibilities"] --> B["Choose deployment and network"]
    B --> C["Control identity and access"]
    C --> D["Collect operational evidence"]
    D --> E["Respond and recover"]
    E --> F["Automate repeatably"]
```

## How To Use This Area

Treat these topics as one production-readiness layer. Security design without monitoring and recovery ownership is incomplete.

## Related Areas

- [[03 Fivetran/Fivetran Learning Map|Fivetran Learning Map]]
- [[03 Fivetran/04 Fivetran with Snowflake and dbt/Fivetran with Snowflake and dbt Overview|Previous: Fivetran with Snowflake and dbt]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/Cost and Consultant Decision-Making Overview|Next: Cost and Consultant Decision-Making]]

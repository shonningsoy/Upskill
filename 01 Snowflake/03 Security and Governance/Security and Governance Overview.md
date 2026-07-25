---
status: hub
platform: Snowflake
area: Security and Governance
tags:
  - snowflake
  - sf-security-governance
  - map
---

# Security and Governance Overview

> [!abstract] Chapter outcome
> Explain Snowflake's layered control model — identity and privileges, row and column enforcement, discovery and metadata, network boundaries, and customer-held encryption control.

## Topics

- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges|12 - RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/13 Row Access Policies|13 - Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies|14 - Column-level Masking Policies]]
- [[01 Snowflake/03 Security and Governance/15 Data Classification|15 - Data Classification]]
- [[01 Snowflake/03 Security and Governance/16 Network Policies and Private Connectivity|16 - Network Policies and Private Connectivity]]
- [[01 Snowflake/03 Security and Governance/17 Tri-Secret Secure and Customer-Managed Keys|17 - Tri-Secret Secure and Customer-Managed Keys]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging|18 - Object Tagging]]

## Chapter Map

```mermaid
flowchart TD
    A["User or workload"] --> B["Network boundary"]
    B --> C["RBAC privileges"]
    C --> D["Row policies"]
    D --> E["Masking policies"]
    F["Classification"] --> G["Object tags"]
    G --> E
    H["Customer-managed key"] --> I["Encryption control"]
    E --> J["Governed result"]
    I --> J

    classDef input fill:#E8F0FE,stroke:#4C6EF5,color:#172B4D
    classDef control fill:#FFF3BF,stroke:#D69E2E,color:#3D2E00
    classDef snowflake fill:#E6FCF5,stroke:#2F9E7B,color:#123C34
    classDef platform fill:#F1F3F5,stroke:#868E96,color:#212529
    classDef output fill:#F3E8FF,stroke:#805AD5,color:#2D1B4E

    class A,H input
    class B,C,D,E,I control
    class F,G snowflake
    class J output
```

## Topic Summaries

| Topic | What it unlocks |
|---|---|
| [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges\|RBAC Roles and Privileges]] | Build auditable least privilege through role hierarchies and separation of duties. This is the foundation. |
| [[01 Snowflake/03 Security and Governance/13 Row Access Policies\|Row Access Policies]] | Enforce regional, departmental, or tenant segregation from shared tables at query time. |
| [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies\|Column-level Masking Policies]] | Show real, partial, hashed, or null values from the same column according to role or context. |
| [[01 Snowflake/03 Security and Governance/15 Data Classification\|Data Classification]] | Discover and label sensitive columns so protection and compliance reporting can scale. Classification finds data; policies protect it. |
| [[01 Snowflake/03 Security and Governance/16 Network Policies and Private Connectivity\|Network Policies and Private Connectivity]] | Control where connections originate and whether traffic uses public or private network paths. |
| [[01 Snowflake/03 Security and Governance/17 Tri-Secret Secure and Customer-Managed Keys\|Tri-Secret Secure and Customer-Managed Keys]] | Give qualified clients customer-held key control and a revocation kill switch. It changes control, not encryption strength. |
| [[01 Snowflake/03 Security and Governance/18 Object Tagging\|Object Tagging]] | Create a shared governance vocabulary for masking, classification, cost attribution, and inventory. |

## How To Use This Area

Read the chapter as layered defense. Start with RBAC, add row and column policies for fine-grained access, use classification and tags to scale governance, then add network and key controls where risk or regulation requires them.

## Related Areas

- [[00 Home/Snowflake Learning Map|Snowflake Learning Map]]
- [[01 Snowflake/01 Core Architecture and Concepts/Core Architecture and Concepts Overview|Core Architecture and Concepts]]
- [[01 Snowflake/04 Data Engineering/Data Engineering Overview|Data Engineering]]
- [[01 Snowflake/08 Enterprise Snowflake in Production/Enterprise Snowflake in Production Overview|Enterprise Snowflake in Production]]

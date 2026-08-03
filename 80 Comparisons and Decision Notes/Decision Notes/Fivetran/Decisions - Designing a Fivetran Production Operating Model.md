---
tags:
  - note-decision
---

# Decisions - Designing a Fivetran Production Operating Model

> Turn managed connectors into an owned production service with explicit controls, evidence, monitoring, and recovery.

## Decision Frame

Fivetran manages connector execution, but the client still needs owners and controls for data approval, identities, network paths, destination access, schema change, freshness, reconciliation, incident response, automation, cost, and vendor management.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Small estate with standard controls | Central platform team owns destinations and shared standards; domain owners own source approval | Keeps governance simple | Avoid one-person administration and undocumented exceptions |
| Many domain-owned connections | Delegated teams within centrally governed RBAC, naming, monitoring, and recovery standards | Scales ownership without losing controls | Higher-level permissions cascade; review effective access |
| Regulated or critical data | Formal production gates, least privilege, end-to-end reconciliation, audit retention, and tested runbooks | Produces demonstrable control evidence | Plan and logging limitations must be verified |
| Repeated configuration across environments | REST API or Terraform through reviewed CI/CD | Improves repeatability and attribution | Protect secrets and state; manage unsupported fields and drift |
| No named owner for alerts and business validation | Do not go live | Managed execution cannot replace operational accountability | Establish RACI and escalation first |

## Questions To Ask

- Who approves source scope and sensitive fields?
- Who owns Fivetran administration, destination access, and credential rotation?
- How is end-to-end freshness measured after downstream transformations?
- Which events are retained, where, and for how long?
- Who decides between retry, pause, table re-sync, or full re-sync?
- How are configuration, plan, connector, source API, and pricing changes reviewed?

## Related Learning Topics

- [[03 Fivetran/05 Security Governance and Production Operations/22 Security Architecture and Shared Responsibility]]
- [[03 Fivetran/05 Security Governance and Production Operations/24 Credentials RBAC SSO SCIM and Service Access]]
- [[03 Fivetran/05 Security Governance and Production Operations/26 Monitoring Logging Freshness and the Platform Connector]]
- [[03 Fivetran/05 Security Governance and Production Operations/27 Incident Response Re-syncs and Recovery Runbooks]]
- [[03 Fivetran/05 Security Governance and Production Operations/28 REST API Terraform and Configuration Automation]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - SaaS vs Hybrid Deployment]]

## Sources To Revisit

- [Fivetran Role-Based Access Control](https://fivetran.com/docs/using-fivetran/fivetran-dashboard/account-settings/role-based-access-control)
- [Fivetran Logs](https://fivetran.com/docs/logs)
- [Fivetran REST API](https://fivetran.com/docs/rest-api)

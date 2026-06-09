---
tags:
  - note-scenario
---

# Scenario - New Regulatory Team Needs Cross-Domain Data

> Client says: "We're setting up a new regulatory reporting team that needs to query both Finance and Risk data in the same report. How do we give them access without over-provisioning?"

## Likely Reasoning Path

1. Confirm the access need is legitimate and permanent (regulatory reporting is ongoing, not a one-off request).
2. Check what roles already exist — likely `DOMAIN_FINANCE_READ` and `DOMAIN_RISK_READ` cover the required schemas.
3. Evaluate options: secondary roles vs. composite role vs. separate reporting views.
4. Prefer a composite role: create `ANALYST_REGULATORY_REPORTING` that inherits both domain read roles.
5. Name it meaningfully so security team can audit it (the role name documents its purpose).
6. Map the composite role to an Entra ID group (`GRP-SF-ANALYST-REGULATORY`) for automated provisioning.
7. Assign a dedicated warehouse (`WH_REGULATORY`) for cost attribution and workload isolation.
8. Document the approval in the organization's access request system (ServiceNow/Sailpoint).
9. Monitor with ACCESS_HISTORY to confirm the team only queries expected objects.

## Consultant Recommendation Shape

Create a purpose-built composite role that inherits from both domain read roles. This gives the regulatory team exactly the access they need — no more, no less. The role name documents the justification, the Entra ID group automates provisioning, and the audit trail shows a single named role for every query. Avoid secondary roles here because regulators will ask "which role granted access?" and you want a clean one-word answer.

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]]
- [[01 Snowflake/06 Cost Management and Operations/31 Account Usage Views]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Secondary Roles vs Composite Roles]]

## Questions To Ask

- Is the cross-domain need permanent or time-limited? (If temporary, consider a time-boxed grant with automated revocation.)
- Do they need all tables in both domains or only specific reporting views? (If the latter, a curated reporting schema may be cleaner.)
- Are there row-level restrictions within the domains? (If yes, Row Access Policies may also be needed.)
- What warehouse should they use — shared or dedicated? (Dedicated = cleaner cost attribution.)

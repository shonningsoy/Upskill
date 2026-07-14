---
tags:
  - note-scenario
---

# Scenario - Bank Wants to Distribute a Governed Snowflake App Across Accounts

> Client says: "We have several Snowflake accounts across Retail, Risk, Finance, and Treasury. A central team wants to package a reusable monitoring or regulatory app and distribute it without breaking local RBAC or data governance."

## Likely Reasoning Path

1. Confirm the app is truly reusable across accounts, not just a single dashboard or one-off workflow.
2. Identify the provider account: usually central platform, risk technology, model governance, or data product team.
3. Identify consumer accounts: departments, subsidiaries, regions, or business units that install and operate their own local app instance.
4. Decide whether distribution should be a private listing rather than Marketplace; internal bank use normally points to private listing.
5. Define app roles such as `app_user`, `app_admin`, and `app_auditor`.
6. Let each consumer account map application roles to local account roles. Do not require every department to copy the same RBAC design.
7. Define references for required consumer-owned data, such as transactions, exposures, customer master, or risk metrics.
8. Prefer binding references to governed views, not raw base tables.
9. Review account-level privileges carefully. If the app asks to create warehouses, execute tasks, or create compute pools, require explicit justification.
10. Decide who pays for app compute and how usage is monitored.
11. Establish provider support: release channels, upgrade communication, logs/events, runbooks, and ownership.
12. Pilot with one or two accounts before broad rollout.

## Consultant Recommendation Shape

Use a Snowflake Native App if the central team is distributing **logic plus UI/workflow**, not just data. Publish it through a private listing, let each receiving account install its own app instance, map application roles into its local RBAC, and bind app references to approved governed views. This gives the bank one standardized product while preserving departmental data boundaries.

Do not position the app as a cross-account bypass. The consumer account remains in control: it chooses whether to install, which privileges to grant, which local objects to expose, and which users can access the app.

## What the Receiver Actually Does

1. Install the app from the private listing.
2. Review requested privileges and references.
3. Grant only the required global privileges.
4. Bind references to approved local objects, preferably governed views.
5. Grant application roles to local account roles.
6. Use the Streamlit UI, procedures, functions, views, or output tables created by the app.
7. Monitor usage, cost, logs/events, and upgrade behavior.

## Why the App Helps

- Standardizes business logic across accounts without copy/paste SQL.
- Lets data stay in the consumer account.
- Preserves local RBAC and policy enforcement.
- Reduces duplicated departmental implementations.
- Gives the central team a release and support model.
- Helps package data, logic, UI, and documentation together.

## When to Skip

- Only one account needs the tool.
- The requirement is only to share a dataset.
- The app would request broad account privileges without strong justification.
- The organization does not yet have ownership for support, upgrades, security review, and cost monitoring.
- A Streamlit app, direct share, dbt package, Snowflake CLI deployment, or Terraform module solves the problem more simply.

## Related Learning Topics

- [[01 Snowflake/07 Ecosystem and Integration/51 Native Apps Framework]]
- [[01 Snowflake/01 Core Architecture and Concepts/04 Data Sharing and Marketplace]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]
- [[01 Snowflake/07 Ecosystem and Integration/48 Snowflake CLI and Terraform Provider]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Snowflake Sharing Pattern]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Direct Share vs Marketplace Listing]]

## Questions To Ask

- Is the central team distributing data, logic, UI, workflow, or a full product?
- Which Snowflake accounts are providers and consumers?
- Are departments allowed to install internal private listings?
- Which local objects does the app need, and who approves those references?
- Can all data access go through governed views?
- Which application roles are needed?
- Which account-level privileges does the app request?
- What is the cost owner for warehouses, compute pools, or tasks created by the app?
- Who supports upgrades, incidents, and compatibility?

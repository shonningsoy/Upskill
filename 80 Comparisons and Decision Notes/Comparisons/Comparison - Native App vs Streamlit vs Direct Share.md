---
tags:
  - note-comparison
---

# Comparison - Native App vs Streamlit vs Direct Share

> Direct shares distribute live data, Streamlit provides an app UI, and Native Apps package a Snowflake-native product for installation across accounts.

## Short Answer

Use **direct sharing** when the job is to expose governed live data to another Snowflake account. Use **Streamlit in Snowflake** when the job is to build an interactive app or dashboard inside an account. Use **Snowflake Native Apps** when the job is to package data, logic, UI, and workflow for repeatable installation across accounts or customers.

## Comparison Table

| Dimension | Direct Share | Streamlit in Snowflake | Snowflake Native App |
|---|---|---|---|
| Primary job | Share live data | Build interactive UI/app | Package and distribute installable product |
| Main consumer | Another Snowflake account | Users in/with access to the account/app | Consumer accounts installing through listing |
| Contains logic? | Mostly shared objects/views | Python UI logic and queries | SQL logic, procedures, functions, UI, data, optional services |
| Distribution | Grant/share/listing | App URL and Snowflake access | Private listing or Marketplace listing |
| Local consumer data binding | Not the core pattern | App queries objects it has access to | References bind approved local objects |
| RBAC model | Share privileges and imported database access | Snowflake roles grant app/use access | Application roles mapped to consumer roles |
| Best fit | Curated data product | Internal interactive tool | Productized multi-account app |
| Complexity | Low to medium | Medium | High |
| Consultant shorthand | "Share the data." | "Build a UI." | "Package the product." |

## Decision Rules

- If the consumer only needs read-only live data, start with direct sharing.
- If one team needs a friendly internal interface, start with Streamlit.
- If many accounts/customers need the same app installed with versioning and local governance, consider Native Apps.
- If the app needs to run against each consumer's local data, Native App references are a strong fit.
- If there is no reusable product lifecycle, Native Apps are likely too much ceremony.
- If the provider must support external customers, plan documentation, upgrades, support, observability, and security review before recommending Native Apps.

## Common Misreads

- **"Streamlit and Native Apps are the same because both can have UI."** Native Apps are a packaging/distribution framework; Streamlit is a UI/app framework.
- **"Native Apps replace direct shares."** Direct shares are still cleaner for simple live data distribution.
- **"A direct share can distribute a full app workflow."** It shares data objects, not a governed installable product.
- **"Native Apps are common for every dashboard."** They are better for repeatable cross-account products, not one-off departmental dashboards.

## Related Learning Topics

- [[01 Snowflake/07 Ecosystem and Integration/51 Native Apps Framework]]
- [[01 Snowflake/01 Core Architecture and Concepts/04 Data Sharing and Marketplace]]
- [[01 Snowflake/07 Ecosystem and Integration/48 Snowflake CLI and Terraform Provider]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Snowflake Sharing Pattern]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Bank Wants to Distribute a Governed Snowflake App Across Accounts]]

## Sources To Revisit

- [Snowflake Docs: About Secure Data Sharing](https://docs.snowflake.com/en/user-guide/data-sharing-intro)
- [Snowflake Docs: About listings](https://docs.snowflake.com/en/collaboration/collaboration-listings-about)
- [Snowflake Docs: Sharing Streamlit in Snowflake apps](https://docs.snowflake.com/en/developer-guide/streamlit/features/sharing-streamlit-apps)
- [Snowflake Docs: About Snowflake Native App Framework](https://docs.snowflake.com/en/developer-guide/native-apps/native-apps-about)
- [Snowflake Docs: Snowflake Native App workflow](https://docs.snowflake.com/en/developer-guide/native-apps/native-apps-workflow)

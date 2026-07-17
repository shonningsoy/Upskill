---
tags:
  - note-decision
---

# Decisions - Choosing a Snowflake Sharing Pattern

> Choose the sharing pattern from the consumer relationship, account ownership, governance needs, and support model.

## Decision Frame

Snowflake sharing is not only a technical access pattern. It is also a data-product and governance decision: who can access the data, who pays for compute, who supports consumers, and how the shared object stays stable.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Known consumer already has Snowflake | Direct share | Fast, live, read-only sharing with consumer compute | Confirm account identity and access scope |
| Consumer does not have Snowflake | Reader account | Provides controlled access without consumer Snowflake licensing/setup | Provider pays/manages compute and account guardrails |
| Dataset should serve many consumers | Listing / Marketplace | More productized onboarding, metadata, terms, and discoverability | Requires data product ownership and support |
| Internal cross-account sharing | Direct share or private listing | Avoids duplicated pipelines across accounts | Keep ownership and revocation clear |
| Consumers need data plus packaged logic, UI, or workflow | Snowflake Native App | Runs a standardized app in each consumer account while preserving local governance | Requires app lifecycle, privilege review, support, and cost ownership |
| Internal bank wants central logic distributed to many departments/accounts | Private Native App listing | Standardizes logic while each account maps local RBAC and binds approved views | Avoid raw table bindings and broad app privileges |
| Sensitive external sharing | Secure views and policy controls | Exposes only approved rows/columns/logic | Validate consumer-visible behavior before sharing |
| Consumer needs to own transformed data | Share curated source, consumer materializes downstream | Keeps source live while allowing consumer-specific models | Creates derived-copy governance/freshness questions |

## Questions To Ask

- Does the consumer already have a Snowflake account?
- Is the audience one known account, several partners, or a broad market?
- Are we sharing only data, or also packaged logic, UI, and workflow?
- Who pays for query compute?
- What data can safely be exposed: raw tables, curated tables, or secure views?
- Who owns documentation, support, freshness, and revocation?
- If this is an app, who owns upgrades, app privileges, references, and consumer-side cost?

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/04 Data Sharing and Marketplace]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]
- [[01 Snowflake/07 Ecosystem and Integration/51 Native Apps Framework]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Direct Share vs Reader Account]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Direct Share vs Marketplace Listing]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Native App vs Streamlit vs Direct Share]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Partner Needs Access to Live Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Customer Without Snowflake Needs Data Access]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Bank Wants to Distribute a Governed Snowflake App Across Accounts]]

## Sources To Revisit

- Snowflake docs: About Secure Data Sharing - https://docs.snowflake.com/en/user-guide/data-sharing-intro
- Snowflake docs: Manage reader accounts - https://docs.snowflake.com/en/user-guide/data-sharing-reader-create
- Snowflake docs: About listings - https://docs.snowflake.com/en/collaboration/collaboration-listings-about
- Snowflake docs: About Snowflake Native App Framework - https://docs.snowflake.com/en/developer-guide/native-apps/native-apps-about

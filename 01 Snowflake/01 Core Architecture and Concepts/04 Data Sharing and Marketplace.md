---
status: seed
platform: Snowflake
area: Core Architecture and Concepts
topic_number: 04
tags:
  - snowflake
  - sf-core-architecture
  - learning
---

# Data Sharing and Marketplace

> Zero-copy sharing of live governed data across accounts. Consultant lens: Enables secure collaboration and data products without file exports or copy-based ETL handoffs.

## Executive Summary

- **What it is:** Snowflake Secure Data Sharing lets a provider expose selected database objects to consumers without physically copying data. Listings and Marketplace make sharing more productized and discoverable.
- **Why it matters:** It reduces file-transfer pipelines, duplicate storage, freshness problems, and partner handoff friction.
- **Mental model:** The provider keeps the live data; the consumer creates a read-only database from a share and queries it with their own access model.
- **Best used when:** Sharing curated data with partners, subsidiaries, customers, internal accounts, or Marketplace consumers.
- **Avoid or reconsider when:** The data is raw/unstable, legal terms are unclear, governance is weak, or the consumer needs independent ownership and transformation of the data.

## What It Can Do

- Share selected tables, views, secure views, and other supported objects across Snowflake accounts.
- Let consumers query live shared data without loading a copied table.
- Support direct shares to known accounts, private/public listings, Marketplace distribution, and reader accounts.
- Allow providers to revoke access or change what is exposed through the share.
- Enable data-product patterns with documentation, terms, sample queries, and usage visibility through listings.

## What It Cannot Do

- Remove the need for data governance, approval, contracts, or access reviews.
- Make raw/internal tables safe to expose by default.
- Let consumers modify shared objects in the provider account.
- Eliminate all cost; consumers still need compute, and reader accounts shift compute responsibility to the provider.
- Automatically solve semantic quality, freshness expectations, or support obligations for shared data products.

## Core Concepts

| Concept | Meaning | Why it matters |
|---|---|---|
| Provider | Snowflake account that owns and shares the data. | Controls source data, exposed objects, and revocation. |
| Consumer | Account or user group that receives and queries shared data. | Queries a read-only imported database from the share. |
| Share | Snowflake object that packages access to selected database objects. | Core mechanism behind direct sharing and listings. |
| Direct share | Share granted directly to known Snowflake account(s). | Best for controlled partner/internal account sharing. |
| Listing | Productized share with metadata, terms, and consumer workflow. | Better for broad/private/public distribution and Marketplace use. |
| Marketplace | Snowflake distribution channel for discovering and consuming listings. | Useful for third-party datasets and data products. |
| Reader account | Provider-created account for a consumer without Snowflake. | Provider manages the account and is responsible for usage costs. |
| Secure view | View designed to protect underlying logic/data exposure. | Common way to share curated data instead of raw tables. |

## How It Works (Simple Flow)

1. A provider identifies the curated objects to share, usually tables or secure views.
2. The provider creates a share or listing and grants access to selected database/schema/object privileges.
3. The provider adds known consumer accounts, publishes a private/public listing, or creates a reader account.
4. A full Snowflake consumer creates a read-only database from the share/listing.
5. The consumer queries the shared objects using their own warehouse compute.
6. For reader accounts, the provider manages the account, users, warehouses, and usage controls.
7. The provider can update shared data in place or revoke access without sending new files.

## Visuals

![[00 Home/assets/snowflake-core-04-data-sharing-marketplace.png]]

- Original local diagram based on Snowflake Secure Data Sharing, listings, Marketplace, and reader-account documentation.

## Readable Snippets

```sql
-- Provider: create a direct share.
create share partner_revenue_share;
```

```sql
-- Provider: grant access to curated objects.
grant usage on database analytics to share partner_revenue_share;
grant usage on schema analytics.curated to share partner_revenue_share;
grant select on table analytics.curated.monthly_revenue
  to share partner_revenue_share;
```

```sql
-- Provider: add a known Snowflake consumer account.
alter share partner_revenue_share
  add accounts = consumer_org.consumer_account;
```

```sql
-- Consumer: create a read-only database from the provider's share.
create database shared_revenue
  from share provider_org.provider_account.partner_revenue_share;
```

```sql
-- Consumer: query the shared data.
select *
from shared_revenue.curated.monthly_revenue;
```

## Consultant Talking Points

- **Client question this answers:** How can we share live data with another business unit, partner, vendor, or customer without building file exports and ingestion pipelines?
- **Trade-offs to mention:** Direct shares are simple for known Snowflake consumers; listings are better for productized distribution; reader accounts help non-Snowflake consumers but increase provider responsibility.
- **Risk or governance angle:** Treat sharing as publishing. Prefer curated secure views, approval workflows, documented definitions, revocation plans, and sensitivity checks before exposing data.
- **Cost/performance angle:** In a direct share, the provider stores the data and the consumer generally pays query compute. In a reader account, the provider is responsible for the reader account's compute usage.

## Common Pitfalls

- Assuming zero-copy sharing means zero governance.
- Sharing raw production tables instead of curated, stable, documented objects.
- Forgetting that reader account compute is the provider's cost responsibility.
- Treating Marketplace listings as always public; listings can also be private/controlled.
- Sharing columns with PII or contract-restricted data without row/column controls or approvals.
- Not setting freshness/support expectations for consumers.

## When to Recommend What (Decision Table)

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| Known partner already has Snowflake | Direct share | Simple, live, read-only sharing; consumer pays compute | Confirm account identity, region, and shared objects |
| Consumer does not have Snowflake | Reader account | Lets them query shared data without becoming a full Snowflake customer | Provider manages users, warehouses, and compute cost |
| Curated data product for many consumers | Listing / Marketplace | Adds product metadata, terms, discoverability, and usage workflow | Requires documentation, support, legal/commercial thinking |
| Internal cross-account sharing | Direct share or private listing | Avoids duplicate pipelines between accounts/business units | Keep ownership and revocation clear |
| Sensitive data needs controlled exposure | Secure views and policy controls | Limits columns/rows/logic exposed to consumers | Test behavior as consumer; avoid raw table leakage |
| Consumer needs to transform and own downstream data | Share curated source, then let consumer materialize derived tables if needed | Separates source ownership from consumer-specific modeling | Derived copies may create freshness/governance questions |

## Related Topics

- [[01 Snowflake/01 Core Architecture and Concepts/Core Architecture and Concepts Overview]]
- [[01 Snowflake/03 Security and Governance/11 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/12 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/13 Column-level Masking Policies]]
- [[01 Snowflake/03 Security and Governance/17 Object Tagging]]
- [[01 Snowflake/03 Security and Governance/15 Network Policies and Private Connectivity]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Direct Share vs Reader Account]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Direct Share vs Marketplace Listing]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Snowflake Sharing Pattern]]

## Questions

- Which consumers need this data, and do they already have Snowflake accounts?
- Are we sharing raw tables, curated tables, or secure views?
- Who approves external sharing and revocation?
- What sensitivity, contractual, regional, or compliance restrictions apply?
- Who pays for compute, support, and monitoring?

## Sources To Revisit

- Snowflake docs: About Secure Data Sharing - https://docs.snowflake.com/en/user-guide/data-sharing-intro
- Snowflake docs: Share secure database objects - https://docs.snowflake.com/en/user-guide/data-sharing-gs
- Snowflake docs: Manage reader accounts - https://docs.snowflake.com/en/user-guide/data-sharing-reader-create
- Snowflake docs: Consume imported data - https://docs.snowflake.com/en/user-guide/data-share-consumers
- Snowflake docs: About listings - https://docs.snowflake.com/en/collaboration/collaboration-listings-about
- Snowflake docs: About Snowflake Marketplace - https://docs.snowflake.com/en/collaboration/collaboration-marketplace-about

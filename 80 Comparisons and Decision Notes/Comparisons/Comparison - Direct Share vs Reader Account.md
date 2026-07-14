---
tags:
  - note-comparison
---

# Comparison - Direct Share vs Reader Account

> Direct share is for consumers with Snowflake; reader account is for consumers without Snowflake.

## Short Answer

Use a **direct share** when the consumer already has a Snowflake account and should use their own compute. Use a **reader account** when the consumer does not have Snowflake and the provider is willing to manage access and usage costs.

## Comparison Table

| Dimension | Direct share | Reader account |
|---|---|---|
| Consumer has Snowflake? | Yes | No |
| Account ownership | Consumer owns their account | Provider creates/manages account |
| Compute cost | Consumer generally pays query compute | Provider is responsible for reader account compute |
| Consumer scope | Can consume from multiple providers | Can consume from the provider that created it |
| Operational burden | Lower for provider | Higher for provider |
| Consultant recommendation | Best default for known Snowflake partners | Use when non-Snowflake access is required and guardrails are ready |

## Decision Rules

- If both parties have Snowflake, start with direct sharing.
- If the consumer has no Snowflake account, consider a reader account.
- For reader accounts, always discuss warehouse sizing, auto-suspend, resource monitors, and usage monitoring.
- Do not use reader accounts casually when the consumer should really own their own Snowflake environment.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/04 Data Sharing and Marketplace]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Partner Needs Access to Live Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Customer Without Snowflake Needs Data Access]]

---
tags:
  - note-scenario
---

# Scenario - Customer Without Snowflake Needs Data Access

> Client says: "A customer does not use Snowflake, but we want them to query the dataset we maintain."

## Likely Reasoning Path

1. Confirm whether the customer truly cannot use their own Snowflake account.
2. Consider a reader account so the provider can expose shared data through a controlled Snowflake environment.
3. Define users, roles, warehouses, and the minimum shared objects needed.
4. Add resource monitors, auto-suspend, and usage review because provider is responsible for compute charges.
5. Decide whether the customer experience is temporary enablement or a long-term data-product support model.

## Consultant Recommendation Shape

Use a reader account only when provider-managed access is acceptable. It solves the "consumer has no Snowflake" problem, but shifts operational and cost responsibility to the provider.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/04 Data Sharing and Marketplace]]
- [[01 Snowflake/01 Core Architecture and Concepts/05 Resource Monitors]]
- [[01 Snowflake/06 Cost Management and Operations/30 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/31 Account Usage Views]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Direct Share vs Reader Account]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Snowflake Sharing Pattern]]

## Questions To Ask

- Who pays for and monitors reader account compute?
- What usage limits should be enforced before access is granted?
- Is a reader account a temporary bridge or the intended long-term access model?

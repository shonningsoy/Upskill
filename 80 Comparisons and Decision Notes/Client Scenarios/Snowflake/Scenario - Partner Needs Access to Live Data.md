---
tags:
  - note-scenario
---

# Scenario - Partner Needs Access to Live Data

> Client says: "A partner needs access to our latest curated dataset every day, and they already use Snowflake."

## Likely Reasoning Path

1. Confirm the partner has a Snowflake account and identify the exact account locator/name.
2. Define the curated objects to expose; prefer stable tables or secure views over raw internals.
3. Create a direct share or private listing depending on whether this is one-off access or a repeatable data product.
4. Confirm consumer compute responsibility and any regional/compliance constraints.
5. Document freshness, definitions, support owner, and revocation process.

## Consultant Recommendation Shape

Start with a direct share for a known Snowflake partner. If the dataset needs packaging, terms, request workflow, or reuse across many consumers, promote the pattern toward a private listing.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/04 Data Sharing and Marketplace]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Direct Share vs Reader Account]]
- [[80 Comparisons and Decision Notes/Comparisons/Snowflake/Comparison - Direct Share vs Marketplace Listing]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake Sharing Pattern]]

## Questions To Ask

- Which exact data objects should the partner see?
- Should the partner see raw columns or curated business-facing fields?
- What are the contractual, privacy, and revocation requirements?

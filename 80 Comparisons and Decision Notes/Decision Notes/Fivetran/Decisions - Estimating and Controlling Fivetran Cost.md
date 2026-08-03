---
tags:
  - note-decision
---

# Decisions - Estimating and Controlling Fivetran Cost

> Forecast Fivetran from observed change behavior and optimize total pipeline cost without weakening required data service levels.

## Decision Frame

Estimate recurring usage from changed row identities by connection and table, then add plan terms, transformations, Snowflake resources, networking, observability, people, and risk. Optimize unnecessary scope and duplication before reducing business-required freshness or history.

## Recommendation Table

| Situation | Recommend | Why | Watch-outs |
|---|---|---|---|
| New source with uncertain change behavior | Representative pilot covering normal and peak activity | Observed changes are more useful than total source rows | Initial sync and trial usage differ from steady state |
| A few tables dominate paid MAR | Investigate keys, churn, history, rollback, and business necessity | Targets material drivers | Growth may be legitimate, not waste |
| Same source is replicated through several connections | Consolidate to one governed raw landing where policy allows | Separate connections count separately | Environment, region, or security isolation may be mandatory |
| History mode drives high row creation | Limit it to justified entities | Preserves history value with narrower cost | Do not weaken audit requirements |
| Fivetran looks cheap but Snowflake or support is expensive | Optimize end-to-end TCO | Vendor usage is only one component | Include reliability and engineering savings fairly |

## Questions To Ask

- What percentage of each important table changes per month?
- Are keys stable, and which tables use history, re-import, or rollback behavior?
- Is the same source replicated through multiple connections or destinations?
- Which usage is free, paid, trial, transformation, or Activation usage under the current contract?
- What Snowflake compute, storage, network, monitoring, and support costs accompany the design?
- Which proposed saving changes a required freshness, history, control, or isolation outcome?

## Related Learning Topics

- [[03 Fivetran/06 Cost and Consultant Decision-Making/29 Monthly Active Rows]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/30 Forecasting Cost Drivers and Usage Optimization]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/31 Destination Cost and End-to-End Total Cost of Ownership]]
- [[03 Fivetran/03 Destination Data History and Schema Change/12 Soft Delete Mode vs History Mode]]
- [[03 Fivetran/04 Fivetran with Snowflake and dbt/18 Snowflake Database Schema Warehouse and Cost Design]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - Soft Delete Mode vs History Mode]]
- [[80 Comparisons and Decision Notes/Comparisons/Fivetran/Comparison - Fivetran vs Custom Ingestion]]

## Sources To Revisit

- [Fivetran Usage-Based Pricing](https://fivetran.com/docs/getting-started/pricing)
- [Fivetran Monitor and Optimize Usage](https://fivetran.com/docs/core-concepts/usage-based-pricing/tracking-and-optimizing-usage)
- [Fivetran Platform Connector](https://fivetran.com/docs/logs/fivetran-platform)

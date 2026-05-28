---
tags:
  - note-decision
---

# Decisions - Choosing Table Retention by Data Criticality

> Retention should follow recovery value, rebuildability, and detection time.

## Decision Frame

Time Travel retention is a recovery design choice. The useful question is not "how much retention can Snowflake support?" but "how much recovery window is worth paying for on this class of data?"

## Recommendation Table

| Data class | Starting recommendation | Why | Watch-outs |
|---|---|---|---|
| Critical production finance/customer data | Permanent table with selective longer retention | High recovery and audit value | Longer retention can increase storage cost |
| Rebuildable staging data | Transient table or short retention | Source can be reloaded | Keep enough window to detect bad loads |
| High-churn tables | Measure before extending retention | Changes can retain more historical data | Current table size alone can mislead |
| Short-lived analyst work | Temporary table | Disposable by nature | Not for shared durable reporting |
| Business history requirements | Model history explicitly | Time Travel is not long-term analytics history | Requires data modeling and governance |

## Questions To Ask

- Can this data be rebuilt from upstream sources?
- How quickly would the team notice a bad load or accidental change?
- What recovery SLA does the business actually need?
- Is this recovery history or business/audit history?

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/30 Account Usage Views]]

## Related Comparisons and Scenarios

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Permanent vs Transient vs Temporary Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Modeled Historical Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Snowflake Costs Spiked After Retention Change]]

---
tags:
  - note-scenario
---

# Scenario - Daily Load Overwrote Good Data

> Client says: "Today's pipeline loaded bad data and replaced yesterday's correct table."

## Likely Reasoning Path

1. Identify when the bad load happened or find the query ID in history.
2. Use Time Travel to query or clone the table before the bad statement/time.
3. Restore side-by-side first, then compare and decide what to merge back.
4. Review retention: was the recovery window long enough for realistic detection?
5. Review permissions and release controls so the same failure is less likely next time.

## Consultant Recommendation Shape

Time Travel is the first recovery tool, but the follow-up is governance and pipeline design. Recovery fixes the incident; controls reduce repeat incidents.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]
- [[01 Snowflake/03 Security and Governance/11 RBAC Roles and Privileges]]
- [[01 Snowflake/06 Cost Management and Operations/30 Account Usage Views]]
- [[01 Snowflake/04 Data Engineering/19 Dynamic Tables]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Fail-safe]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Modeled Historical Data]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing Table Retention by Data Criticality]]

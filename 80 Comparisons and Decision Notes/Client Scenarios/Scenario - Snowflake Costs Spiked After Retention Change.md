# Scenario - Snowflake Costs Spiked After Retention Change

> Client says: "We changed retention to 30 days and storage costs jumped."

## Likely Reasoning Path

1. Identify which databases, schemas, or tables inherited the new retention setting.
2. Look for high-churn tables: daily rebuilds, frequent `MERGE`, `UPDATE`, `DELETE`, or `CREATE OR REPLACE`.
3. Separate critical data from rebuildable staging/intermediate data.
4. Shorten retention or use transient tables where data can be rebuilt safely.
5. Keep longer retention only where recovery value justifies the cost.

## Consultant Recommendation Shape

The client likely changed a broad default instead of making a data-class decision. Retention should be selective and tied to recovery requirements.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]
- [[01 Snowflake/06 Cost Management and Operations/29 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/30 Account Usage Views]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Permanent vs Transient vs Temporary Tables]]
- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Time Travel vs Modeled Historical Data]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing Table Retention by Data Criticality]]

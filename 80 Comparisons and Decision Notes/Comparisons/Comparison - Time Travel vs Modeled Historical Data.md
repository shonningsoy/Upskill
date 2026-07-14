---
tags:
  - note-comparison
---

# Comparison - Time Travel vs Modeled Historical Data

> Recovery history and business history are different requirements.

## Short Answer

Use **Time Travel** when the team needs to recover or inspect a recent previous state. Use **modeled historical data** when the business needs durable analytics history, such as customer status over time or month-end balances.

## Comparison Table

| Dimension | Time Travel | Modeled historical data |
|---|---|---|
| Primary purpose | Operational recovery | Analytics and audit history |
| Time horizon | Configured retention window | As long as the model keeps history |
| Access pattern | Point-in-time recovery/query | Normal reporting and analysis |
| Examples | Undo bad load, restore dropped table | SCD Type 2, snapshots, audit facts |
| Consultant warning | Not a long-term history strategy | Requires intentional modeling and governance |

## Decision Rules

- If the question is "what did the table look like before the bad load?", use Time Travel.
- If the question is "what was the customer's status at each month-end?", model history explicitly.
- Do not sell 90-day Time Travel as a replacement for audit or regulatory history.
- Use Time Travel to repair mistakes, not to power recurring historical reporting.

## Related Learning Topics

- [[01 Snowflake/01 Core Architecture and Concepts/03 Time Travel and Fail-safe]]
- [[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Daily Load Overwrote Good Data]]
- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Snowflake Costs Spiked After Retention Change]]

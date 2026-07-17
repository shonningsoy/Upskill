---
tags:
  - note-scenario
---

# Scenario - Dashboard Published From Stale Source Data

> Client says: "The executive dashboard refreshed this morning, but later we discovered the upstream source had not loaded overnight."

## Likely Reasoning Path

1. Separate successful dbt execution from successful source arrival.
2. Identify which raw/source tables feed the published dashboard.
3. Add source declarations for the upstream tables if they do not already exist.
4. Define freshness thresholds that reflect the business SLA, not just a technical schedule.
5. Use exposures to connect the dashboard to its upstream mart models.
6. Run freshness checks before building or publishing downstream models.
7. Route freshness failures to the team that can act on ingestion, not only the analytics team.

## Consultant Recommendation Shape

The dashboard problem is not only a BI problem. The dbt project needs to know which upstream sources must be fresh before the mart is trusted. Add source freshness checks and connect the dashboard through exposures so a stale source can be traced to affected business outputs.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Dashboard depends on raw data that may arrive late | Add source freshness checks |
| Dashboard is important to executives or regulators | Define an exposure with owner and maturity |
| Freshness failure should block publication | Run `dbt source freshness` before downstream `dbt build` |
| Freshness failure should warn but not block | Use alerting and visible status, with documented tolerance |
| Multiple dashboards use the same mart | Use lineage and exposures for impact analysis |

## Watch-outs

- A green `dbt run` does not prove the source data arrived.
- Freshness thresholds should match business expectations, not only ingestion schedules.
- Freshness checks need a reliable loaded timestamp or business timestamp.
- Exposures do not monitor dashboards by themselves; they declare dependency and ownership.
- Stale data can be more dangerous than failed data because users may trust it.

## Related Learning Topics

- [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness]]
- [[02 dbt/01 Core Concepts and Project Structure/10 Documentation Lineage and Exposures]]
- [[02 dbt/01 Core Concepts and Project Structure/06 Commands and Artifacts]]
- [[02 dbt/03 Testing Documentation and Data Quality/23 Source Freshness and SLA Monitoring]]
- [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results]]

## Related Decision Notes

- No related decision note yet.

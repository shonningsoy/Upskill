---
tags:
  - note-comparison
---

# Comparison - Full CI vs Slim CI

> Full CI maximizes simple confidence by rebuilding the project; Slim CI trades some simplicity for faster feedback by building changed resources and their impact scope.

## Short Answer

Use **full CI** when the project is small enough to rebuild cheaply, artifact state is not yet trustworthy, or the team needs the simplest possible validation story.

Use **Slim CI** when the dbt DAG is large, production artifacts are reliable, PR feedback must stay fast, and the team understands state selection, deferral, and mixed-environment tests.

## Comparison Table

| Dimension | Full CI | Slim CI |
|---|---|---|
| Primary purpose | Validate the whole project for a candidate commit | Validate changed resources and downstream impact efficiently |
| Main mechanism | `dbt build --target ci` or equivalent full selector | State comparison, graph selection, deferral, isolated CI target |
| Typical selector | Entire project | `state:modified+` with `--state` and often `--defer` |
| Setup complexity | Lower | Higher |
| Runtime and cost | Higher as project grows | Lower for narrow changes, but broad changes can still select much of the DAG |
| Evidence claim | "This project built under this CI context" | "This selected impact scope passed against this comparison/deferred state" |
| Artifact dependency | Not required | Requires trusted, compatible prior `manifest.json` |
| Data isolation | Easier to reason about if all objects build in CI | Can mix CI-built children with deferred production or approved parents |
| Incremental-model confidence | Often still limited if CI target is empty | Also limited unless target state is cloned or prepared |
| Best fit | Small projects, early teams, strict isolation, uncertain artifacts | Large projects, mature CI, trusted production state, cost-sensitive PR validation |
| Main risk | Slow, expensive, and eventually avoided by developers | False confidence if state, selector, deferral, or mixed data is misunderstood |
| Consultant recommendation | Start here if affordable | Adopt when full CI becomes too slow and artifact ownership is mature |

## Decision Rules

- Start with full CI when the project is small, because it is easier to explain and operate.
- Move to Slim CI when full CI feedback time or Snowflake cost becomes a real delivery constraint.
- Do not adopt Slim CI until the production comparison manifest is trustworthy, versioned, and retained outside `target/`.
- Use `dbt ls` to preview Slim CI scope before treating it as cheap.
- Add downstream expansion, usually `state:modified+`, so changed upstream logic validates affected children.
- Treat a broad Slim CI selection after a central macro or staging-model change as expected impact, not a failure of Slim CI.
- For changed incremental logic, consider Snowflake clones or other realistic target-state preparation; an empty CI schema may only test initial creation.
- Keep CI write privileges away from production regardless of CI strategy.

## Related Learning Topics

- [[02 dbt/05 Deployment CI CD and Operations/42 CI Jobs and Slim CI]]
- [[02 dbt/05 Deployment CI CD and Operations/41 Dev CI Staging and Prod Environments]]
- [[02 dbt/05 Deployment CI CD and Operations/45 Artifacts Logs and Run Results]]
- [[02 dbt/04 Incremental Processing and Performance/36 Model Selection State and Deferral]]
- [[02 dbt/04 Incremental Processing and Performance/31 Incremental Models and Unique Keys]]

## Related Decisions and Scenarios

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Environment and Credential Strategy]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Production Deployment Pattern]]
- [[80 Comparisons and Decision Notes/Client Scenarios/dbt/Scenario - Team Cannot Tell Which Dashboards a Model Change Will Break]]

## Sources To Revisit

- [dbt Developer Hub - Continuous integration jobs](https://docs.getdbt.com/docs/deploy/ci-jobs)
- [dbt Developer Hub - Advanced CI](https://docs.getdbt.com/docs/deploy/advanced-ci)
- [dbt Developer Hub - State selection](https://docs.getdbt.com/reference/node-selection/state-selection)
- [dbt Developer Hub - Defer to another environment](https://docs.getdbt.com/reference/node-selection/defer)

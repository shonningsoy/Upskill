---
tags:
  - note-comparison
---

# Comparison - dbt Packages vs Project Dependencies

> A package shares code that becomes part of the consumer's runtime; a project dependency shares a producer-owned dataset through a governed public interface.

## Short Answer

Use a **package dependency** when several projects should reuse macros, tests, configuration patterns, or transformation code.

Use a **project dependency** when one independently operated dbt project should consume a public model that another team already builds and supports.

The memorable rule is:

> **Packages distribute implementation; project dependencies publish interfaces.**

## Comparison Table

| Dimension | Package dependency | Project dependency |
|---|---|---|
| Primary purpose | Code reuse | Cross-team data-product consumption |
| Mental model | Software library | API returning a dataset |
| Shared asset | Package source code | Producer's public built relation |
| Configuration file | `packages.yml`, or compatible package entry in `dependencies.yml` | `projects:` entry in `dependencies.yml` |
| Installation | Downloaded by `dbt deps` | Resolved through dbt platform metadata |
| Parsed by consumer | Yes | No |
| Producer models built by consumer | Possible and often expected when models are packaged | No |
| Runtime configuration | Consumer may need package variables, environment variables, dispatch, or target configuration | Producer owns its runtime configuration |
| Reference pattern | Package macros or normal packaged resources | Two-argument `ref('<project>', '<model>')` |
| Interface boundary | Code version and package API | Public model, contract, version, owner, and dataset |
| Execution ownership | Usually consumer | Producer |
| Warehouse cost ownership | Consumer can incur packaged model cost | Producer pays to build public output; consumer pays downstream |
| Project size impact | Adds resources and parsing | Keeps consumer parse scope narrow |
| dbt Mesh role | Does not create the normal Mesh data-product dependency | Foundational Mesh mechanism |
| Best example | `dbt_utils`, shared generic tests, company macros | Risk consuming Finance's certified exposure model |
| Main failure mode | Unintended builds, configuration coupling, slow parsing, blurred ownership | Stale producer output, weak SLA, unsafe environment resolution, breaking interface change |

## Decision Rules

- Use packages for utility macros, reusable tests, standardized scaffolding, and transformations the consumer is expected to execute.
- Use project dependencies when the producer must retain responsibility for logic, build schedule, quality, support, and warehouse cost.
- Do not install an internal domain project as a package merely to reference its final models.
- Require project-dependent models to be deliberate public interfaces rather than arbitrary staging or intermediate nodes.
- Add contracts, tests, documentation, ownership, versions, and deprecation in proportion to interface criticality.
- Keep Snowflake grants and security policies separate from dbt model access.
- Retain `packages.yml` when package specifications require dynamic Jinja or a package mechanism not supported in `dependencies.yml`.
- Consider internal-project packaging only as an exception for unified deployment or temporary coordinated testing.
- If coordinated testing is constantly required across the boundary, reconsider whether the projects are sufficiently independent.
- Do not choose project dependencies without an eligible dbt platform plan and reliable producer deployment metadata.

## Example

### Reuse code through a package

```yaml
# packages.yml

packages:
  - package: dbt-labs/dbt_utils
    version: 1.3.3
```

```sql
select
    {{ dbt_utils.generate_surrogate_key(['customer_id', 'account_id']) }}
        as customer_account_key
from {{ ref('stg_accounts') }}
```

The consumer downloads and runs the package code inside its own project context.

### Consume a dataset through a project dependency

```yaml
# dependencies.yml

projects:
  - name: finance
```

```sql
select *
from {{ ref('finance', 'fct_certified_revenue') }}
```

The Finance project owns and builds `fct_certified_revenue`. The consumer receives metadata that resolves the existing relation, not Finance's project source code.

## Consultant Questions

- Does the client want to reuse logic or consume an already-built domain output?
- Who should own execution, data quality, support, and warehouse cost?
- Should the consumer be able to build or change the upstream resource?
- Is the dependency a stable interface or tightly coupled shared implementation?
- Does the producer have a successful deployment environment and published metadata?
- Is the model intentionally public, contracted, documented, tested, and versioned where necessary?
- Are staging and production resolution rules safe for sensitive data?
- Will packaging the internal project create unnecessary parse scope, runtime variables, or accidental builds?
- Does frequent cross-project branch coordination reveal that the proposed boundary is wrong?

## Related Learning Topics

- [[02 dbt/06 Governance Semantic Layer and Mesh/54 dbt Mesh and Project Dependencies]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/51 Model Access]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/52 Model Contracts]]
- [[02 dbt/06 Governance Semantic Layer and Mesh/53 Model Versions and Deprecation]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/60 Package Fundamentals]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/61 packages.yml vs dependencies.yml]]
- [[02 dbt/07 Packages Macros and Advanced Reuse/66 Package Governance]]

## Related Decisions and Scenarios

- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Versioning and Deprecating a dbt Model]]
- [[80 Comparisons and Decision Notes/Decision Notes/dbt/Decisions - Choosing a dbt Environment and Credential Strategy]]

## Sources To Revisit

- [dbt Developer Hub - Project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies)
- [dbt Developer Hub - About dbt Mesh](https://docs.getdbt.com/docs/mesh/about-mesh)
- [dbt Developer Hub - Package management](https://docs.getdbt.com/docs/build/packages)
- [dbt Developer Hub - Model access](https://docs.getdbt.com/docs/mesh/govern/model-access)

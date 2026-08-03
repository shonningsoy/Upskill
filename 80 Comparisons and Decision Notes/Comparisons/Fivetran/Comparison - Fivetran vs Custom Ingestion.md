---
tags:
  - note-comparison
---

# Comparison - Fivetran vs Custom Ingestion

## Short Answer

Use **Fivetran** when a supported connector meets the required coverage, latency, security, and recovery needs and the client values managed maintenance.

Use **custom ingestion** when the source or delivery contract needs behavior a managed connector cannot provide and the client accepts full engineering ownership.

## Comparison Table

| Dimension | Fivetran | Custom ingestion |
|---|---|---|
| Primary purpose | Standardized managed data movement | Workload-specific extraction and loading |
| Strengths | Faster onboarding, managed source changes, common monitoring and recovery patterns | Complete control over extraction logic, runtime, data shape, and deployment |
| Limits | Connector-specific coverage and behavior; vendor and plan boundaries | Client owns code, tests, upgrades, security, monitoring, replay, and support |
| Cost considerations | MAR, plan features, destination cost, and vendor commitment | Engineering build/run cost, infrastructure, incidents, and long-term maintenance |
| Governance considerations | Requires connector assessment, scoped access, data minimization, and vendor review | Requires SDLC controls, dependency review, secrets, audit evidence, and named support ownership |
| Consultant recommendation | Default for conventional supported analytical sources | Exception path for material unmet requirements |

## Decision Rules

- Do not treat catalog presence as proof of fit; verify required objects, fields, history, deletes, keys, latency, and controls.
- Prefer Fivetran when the managed service removes more lifecycle burden than it adds in cost and vendor dependency.
- Prefer custom ingestion only when the unmet requirement is material and the client can operate the pipeline throughout its life.
- Keep a governed exception path instead of forcing every source through one standard.

## Related Learning Topics

- [[03 Fivetran/01 Foundations and Platform Mental Model/01 What Fivetran Is and Is Not]]
- [[03 Fivetran/01 Foundations and Platform Mental Model/05 When to Recommend Fivetran]]
- [[03 Fivetran/02 Connectors and Sync Behavior/06 Connector Types Coverage and Maturity]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/31 Destination Cost and End-to-End Total Cost of Ownership]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/32 Fivetran Recommendation and Enterprise Adoption Framework]]

## Related Scenarios

- No dedicated scenario note yet.

## Sources To Revisit

- [Fivetran Core Concepts](https://fivetran.com/docs/core-concepts)
- [Fivetran Connector SDK](https://fivetran.com/docs/connector-sdk)
- [Fivetran Usage-Based Pricing](https://fivetran.com/docs/getting-started/pricing)

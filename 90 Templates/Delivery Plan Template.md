---
status: draft
note_type: delivery-plan
epistemic_status: planned
use_as: delivery-structure
validation_status: unvalidated
last_verified:
sensitivity: public-generalized
tags:
  - planning
---

# Delivery Plan - Outcome

> Translate a sufficiently understood technical direction into sequenced, testable work while keeping uncertainty visible.

> [!note] Working-output template
> Use this to structure a chat response or draft. Do not save situational backlog content to the vault unless explicitly asked and a generalized durable destination is agreed.

## Objective and Expected Outcome

- **Objective:**
- **User or business outcome:**
- **Success measures:**

## Scope and Non-scope

### In scope

-

### Non-scope

-

## Technical Direction

- **Chosen or proposed approach:**
- **Architecture decision link:**
- **Unresolved technical decisions:**
- **Assumptions:**

## Discovery Questions and Spikes

| Question or uncertainty | Resolution method | Owner role or team archetype | Blocks |
|---|---|---|---|
|  |  |  |  |

Create a technical spike instead of hiding material uncertainty inside an implementation story.

## Dependencies and Sequencing

```mermaid
flowchart LR
    A[Discovery or spike] --> B[Foundation]
    B --> C[Implementation]
    C --> D[Validation and rollout]
```

| Dependency | Needed by | Risk if delayed |
|---|---|---|
|  |  |  |

## Epics, Stories, and Tasks

### Epic - Outcome

#### Story - User or system capability

**As a:** ...

**I want:** ...

**So that:** ...

**Acceptance criteria**

- [ ] Given ..., when ..., then ...
- [ ] Failure and recovery behavior is defined and tested.
- [ ] Required evidence and operational documentation are produced.

**Dependencies:**

**Estimate assumptions:**

**Risks:**

## Non-functional Requirements

| Area | Requirement | Verification |
|---|---|---|
| Security and access |  |  |
| Reliability and recovery |  |  |
| Performance and scale |  |  |
| Data quality and reconciliation |  |  |
| Observability and alerting |  |  |
| Cost and capacity |  |  |
| Auditability and governance |  |  |

## Test and Reconciliation Plan

- **Unit and component testing:**
- **Integration and end-to-end testing:**
- **Data-quality and reconciliation checks:**
- **Performance and failure testing:**
- **Evidence required for acceptance:**

## Rollout, Rollback, and Recovery

- **Release strategy:**
- **Migration or backfill:**
- **Rollback trigger and procedure:**
- **Recovery and replay:**
- **Post-release validation:**

## Operational Ownership

- **Service owner role or team archetype:**
- **Monitoring and alert response:**
- **Runbook:**
- **Support and escalation:**
- **Ongoing cost and capacity review:**

## Risks and Open Decisions

| Risk or decision | Impact | Mitigation or owner role/team archetype | Due |
|---|---|---|---|
|  |  |  |  |

## Evidence and Sources

- Link the architecture decision, relevant vault notes, spike results, measurements, and current primary sources that materially shaped the plan.
- Distinguish verified constraints from planning assumptions.

## Definition of Done

- [ ] Acceptance criteria and non-functional requirements are met.
- [ ] Tests and reconciliation evidence are complete.
- [ ] Monitoring, alerts, and runbooks are operational.
- [ ] Security, governance, and required approvals are complete.
- [ ] Rollback and recovery have been validated.
- [ ] Ownership and support handoff are explicit.

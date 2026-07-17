---
tags:
  - note-comparison
---

# Comparison - On-demand Cortex Inference vs Provisioned Throughput

> On-demand Cortex inference uses managed capacity per request; Provisioned Throughput reserves model capacity with PTUs. Consultant lens: start on-demand, reserve only after demand is measured.

## Short Answer

Use **on-demand Cortex inference** for exploration, low-to-moderate usage, irregular workloads, and early production adoption. Use **Provisioned Throughput** when a supported model powers predictable, business-critical, high-volume inference and the organization is willing to pay for reserved capacity.

## Comparison Table

| Dimension | On-demand Cortex inference | Provisioned Throughput |
|---|---|---|
| Primary job | Call Cortex AI functions or APIs using managed shared capacity | Reserve managed inference capacity for supported models |
| Commitment | No reserved PTU term | PTUs allocated for a defined term |
| Cost behavior | Usage-driven by function/model calls and tokens | Charges for allocated PTUs regardless of actual usage |
| Best fit | Experiments, sporadic jobs, early rollout, uncertain demand | Predictable production traffic, strict processing windows, high-volume API usage |
| Setup overhead | Lower | Higher; requires privilege, object creation, sizing, approval/support process |
| Scaling decision | Let standard managed service handle typical demand | Size PTUs based on measured request/token/concurrency profile |
| Governance focus | Model/function access, usage visibility, budgets, prompt/output controls | Same, plus capacity ownership, term dates, renewal, and utilization review |
| Failure mode | Variable latency/capacity during busy periods or bursts | Paying for unused capacity or under-sizing reserved capacity |
| Bank example | Small compliance summarization pilot | Enterprise analyst assistant used by hundreds of staff every morning |
| Consultant shorthand | "Use the managed default." | "Reserve capacity for a proven workload." |

## Decision Rules

- Start with **on-demand inference** until volume, token size, latency, concurrency, and business criticality are understood.
- Consider **Provisioned Throughput** only when the workload is stable enough to size and valuable enough to reserve capacity for.
- If usage is bursty, seasonal, or experimental, reserved capacity can waste money.
- If a daily AI job must finish inside a fixed window or a user-facing app has predictable heavy traffic, Provisioned Throughput may be worth evaluating.
- Provisioned Throughput does not make the model smarter or safer; it changes capacity planning.
- Track utilization and renewal dates, because reserved throughput does not automatically solve lifecycle or cost ownership.

## Common Misreads

- **"Provisioned means better answers."** It reserves capacity; answer quality still depends on model, prompt, data, retrieval, and evaluation.
- **"Provisioned Throughput is the default production pattern."** Many production AI workloads are fine with on-demand managed inference.
- **"If latency is bad, reserve PTUs immediately."** First measure request shape, prompt size, model choice, retrieval overhead, and application behavior.
- **"Reserved capacity controls spend automatically."** It can make spend more predictable, but unused PTUs are still paid for.
- **"Provisioned Throughput removes governance concerns."** Access, region, model approval, logging, and human review still matter.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/43 Cortex Fine-tuning, Provisioned Throughput, and Model Lifecycle]]
- [[01 Snowflake/05 Advanced Analytics and AI/39 AI Governance, Guardrails, Observability, and Cost]]
- [[01 Snowflake/05 Advanced Analytics and AI/33 Cortex AI Functions]]
- [[01 Snowflake/05 Advanced Analytics and AI/38 Cortex Agents and CoWork]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]
- [[01 Snowflake/06 Cost Management and Operations/47 Budgets]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake AI and ML Pattern]]
- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Diagnosing Snowflake Spend Increases]]

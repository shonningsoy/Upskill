---
tags:
  - note-comparison
---

# Comparison - Warehouse Inference vs SPCS Model Serving

> Warehouse inference scores data through Snowflake SQL/warehouse workflows; SPCS model serving runs a containerized service or endpoint for live/custom inference.

## Short Answer

Use **warehouse inference** when the model should score many Snowflake rows in batch or inside analytical pipelines. Use **SPCS model serving** when the model needs low-latency application calls, GPU/custom runtime support, or a service endpoint.

## Comparison Table

| Dimension | Warehouse inference | SPCS model serving |
|---|---|---|
| Core pattern | SQL or Python calls a registered model over tables | App or workflow calls a containerized model service |
| Compute | Virtual warehouse | Snowpark Container Services compute pool |
| Best fit | Batch scoring, enrichment tables, scheduled predictions, analytics workflows | Real-time prediction APIs, custom runtime, GPU, long-running service |
| Latency target | Throughput-oriented | Request/response-oriented |
| Operational complexity | Lower; fits Snowflake SQL and warehouse operations | Higher; requires service, endpoint, compute-pool, and scaling design |
| Cost surface | Warehouse credits while queries run | Compute-pool/service cost while service runs or scales |
| Consumer | SQL users, Snowpark jobs, dynamic tables, pipelines | Applications, APIs, notebooks, services |
| Governance focus | Model privilege, role, query cost, result persistence | Endpoint access, service role, image/runtime, secrets, network exposure |
| Consultant shorthand | "Score the table." | "Serve the model." |

## Decision Rules

- If predictions feed a table, dashboard, campaign list, or scheduled analytics workflow, start with **warehouse inference**.
- If an application needs a prediction during a user interaction, evaluate **SPCS model serving**.
- If the model needs GPU, non-standard dependencies, or a custom container runtime, SPCS is more likely to fit.
- If latency is not user-facing, prefer batch unless there is a strong reason to operate an always-available service.
- Persist batch predictions when many consumers need the same result; avoid rescoring the same rows repeatedly.
- Treat SPCS as a product/service surface: it needs endpoint security, autoscaling rules, monitoring, cost controls, and ownership.

## Common Misreads

- **"Real-time sounds more modern."** Real-time is only better when the business process needs it.
- **"The warehouse is suspended, so inference is free."** SPCS services and compute pools have their own cost surface.
- **"Warehouse inference cannot be production."** Batch scoring is often the cleanest production pattern for analytics use cases.
- **"SPCS is only for ML."** It is a broader container runtime; model serving is one use case.
- **"The registry chooses the serving pattern."** The Model Registry stores and versions the model; the workload shape chooses batch vs service.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/35 ML Model Registry]]
- [[01 Snowflake/05 Advanced Analytics and AI/36 Snowflake Notebooks]]
- [[01 Snowflake/05 Advanced Analytics and AI/32 Snowpark]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - A Churn Model Is Stuck in a Notebook]]

---
tags:
  - note-scenario
---

# Scenario - A Churn Model Is Stuck in a Notebook

> Client says: "A data scientist built a promising churn model in a notebook. The business now wants weekly scoring, monitoring, and maybe a live app prediction."

## Likely Reasoning Path

1. Confirm whether the notebook is exploratory evidence or already acting as hidden production logic.
2. Review training data, feature definitions, evaluation metrics, and business approval criteria.
3. Register the approved model version in the Snowflake Model Registry with a meaningful signature and metrics.
4. Decide whether scoring is batch or real-time.
5. Use warehouse inference for weekly or daily table scoring.
6. Use SPCS model serving only if an app needs low-latency predictions, GPU/custom runtime, or endpoint-style access.
7. Add monitoring for inference volume, cost, drift, errors, and business outcome quality.
8. Define rollback and retraining ownership before the model becomes business-critical.

## Consultant Recommendation Shape

Keep the notebook as the development and explanation artifact, but do not let it remain the production boundary. Promote the model into the registry, choose the simplest inference pattern that fits the business need, and create an operating model around monitoring and rollback.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Weekly customer-table scoring | Model Registry plus warehouse inference |
| Live app prediction during a customer interaction | Model Registry plus SPCS model serving |
| Model is still exploratory | Keep in Snowflake Notebook and improve evaluation |
| Business wants predictions in dashboards | Persist batch predictions rather than rescoring interactively |
| Model affects high-impact decisions | Add formal review, explainability, and human oversight |

## Watch-outs

- A strong notebook demo is not proof of production readiness.
- Training-time and inference-time feature logic must match.
- Hardcoded model versions can make rollback harder.
- Real-time serving is not automatically better than batch scoring.
- Predictions can be sensitive even if the source table is already governed.
- Monitoring should include quality drift and business outcomes, not only technical errors.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/36 Snowflake Notebooks]]
- [[01 Snowflake/05 Advanced Analytics and AI/35 ML Model Registry]]
- [[01 Snowflake/05 Advanced Analytics and AI/32 Snowpark]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Warehouse Inference vs SPCS Model Serving]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Warehouse Strategy by Workload Type]]

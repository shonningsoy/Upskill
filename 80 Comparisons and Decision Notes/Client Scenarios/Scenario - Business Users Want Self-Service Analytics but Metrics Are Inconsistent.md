---
tags:
  - note-scenario
---

# Scenario - Business Users Want Self-Service Analytics but Metrics Are Inconsistent

> Client says: "We want a chat interface where business users can ask revenue and customer questions, but Finance, Sales, and Product define the metrics differently."

## Likely Reasoning Path

1. Separate the desire for a natural-language interface from the unresolved metric-definition problem.
2. Identify the first narrow domain where business value is high and definitions can be agreed.
3. Assign metric ownership before building the user experience.
4. Model the approved dimensions, facts, metrics, joins, synonyms, and examples in Semantic Views.
5. Add verified queries for the top real user questions.
6. Evaluate generated SQL before exposing broad access.
7. Start with a controlled pilot and expand only after logs, failures, and cost are understood.

## Consultant Recommendation Shape

Do not start by "turning on a chatbot for the warehouse." Start with semantic governance. Cortex Analyst is a good fit only after the client agrees what the business terms mean and can test whether generated SQL matches those definitions.

## What To Recommend

| Situation | Recommendation |
|---|---|
| Metric definitions are disputed | Run a metric-definition and ownership workshop first |
| One domain has clear ownership | Build a narrow Semantic View and pilot Cortex Analyst there |
| Users need certified recurring KPIs | Keep official dashboards and semantic definitions as the source of truth |
| Users need ad hoc follow-up questions | Add Cortex Analyst after verified-query evaluation |
| Sensitive data is involved | Design RBAC, masking, row access, and logging review before rollout |

## Watch-outs

- A natural-language interface makes semantic disagreement more visible; it does not resolve it.
- Generated SQL can look plausible even when it uses the wrong business definition.
- Verified queries should come from real business questions, not demo-only examples.
- Logs may contain sensitive questions, generated SQL, or business context.
- Broad warehouse access is not the same as permission to expose every metric conversationally.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/34 Cortex Analyst]]
- [[01 Snowflake/05 Advanced Analytics and AI/33 Cortex AI Functions]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/13 Row Access Policies]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Comparisons/Comparison - Cortex Analyst vs Cortex AI Functions]]
- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Row-Level Data Isolation Strategy]]

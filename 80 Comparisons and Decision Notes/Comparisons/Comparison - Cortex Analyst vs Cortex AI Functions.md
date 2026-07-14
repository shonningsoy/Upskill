---
tags:
  - note-comparison
---

# Comparison - Cortex Analyst vs Cortex AI Functions

> Cortex Analyst answers natural-language metric questions by generating SQL; Cortex AI Functions apply managed AI inference to text, files, media, or rows.

## Short Answer

Use **Cortex Analyst** when the user asks business questions over governed structured data and metrics. Use **Cortex AI Functions** when the workload needs AI transformation or interpretation, such as classification, summarization, extraction, translation, sentiment, embeddings, or generation.

## Comparison Table

| Dimension | Cortex Analyst | Cortex AI Functions |
|---|---|---|
| Primary job | Natural-language-to-SQL over structured data | Managed AI inference inside SQL/Python workflows |
| Main input | User question plus Semantic View or semantic model | Text, documents, images, audio, video, rows, or prompts |
| Main output | SQL, answer text, suggestions, or clarification | Classification, summary, extraction, translation, sentiment, embedding, or generated text |
| Semantic dependency | Requires a modeled semantic layer for reliable answers | Requires prompt/task design and representative evaluation |
| Best fit | Self-service analytics over governed metrics | Enriching or interpreting unstructured/semi-structured data |
| Governance focus | Metric definitions, joins, Semantic View privileges, generated SQL review | Sensitive prompt inputs, model access, output validation, token/call cost |
| Cost surface | Analyst message processing plus SQL execution | Function/model usage plus any query/warehouse cost |
| Failure mode | Correct SQL shape but wrong business interpretation | Plausible output that may be wrong, incomplete, biased, or inconsistent |
| Consultant shorthand | "Ask the data a governed metric question." | "Use AI to transform or interpret content." |

## Decision Rules

- If the user asks "What was revenue by region last quarter?", think **Cortex Analyst**.
- If the workload says "Summarize these tickets and classify the complaint type," think **Cortex AI Functions**.
- If the question requires disputed metrics or joins, fix the semantic layer before exposing Cortex Analyst.
- If the output will feed decisions or automation, evaluate Cortex AI Function quality before trusting the result.
- Do not use Cortex Analyst as a general chatbot over every table; keep the semantic domain focused.
- Do not use Cortex AI Functions to replace deterministic SQL rules when the required logic is exact.

## Common Misreads

- **"Both say Cortex, so they are interchangeable."** The user experience and operating model are very different.
- **"Cortex Analyst can answer any warehouse question."** It needs a semantic contract and a SQL-answerable question.
- **"Cortex AI Functions create governed metrics."** They can enrich data, but metric governance still belongs in modeling/semantic layers.
- **"Natural language means no governance."** Both patterns increase the need for prompt, access, logging, and cost controls.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/34 Cortex Analyst]]
- [[01 Snowflake/05 Advanced Analytics and AI/33 Cortex AI Functions]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - Business Users Want Self-Service Analytics but Metrics Are Inconsistent]]

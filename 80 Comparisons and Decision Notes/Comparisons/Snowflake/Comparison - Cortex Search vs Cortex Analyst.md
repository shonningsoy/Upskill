---
tags:
  - note-comparison
---

# Comparison - Cortex Search vs Cortex Analyst

> Cortex Search retrieves relevant text or document context; Cortex Analyst answers natural-language questions over governed structured data. Consultant lens: text search and metric analytics are different jobs.

## Short Answer

Use **Cortex Search** when users need semantic search or RAG over documents, policies, contracts, logs, transcripts, tickets, or other text-heavy content. Use **Cortex Analyst** when users ask business questions over structured tables through a governed semantic model or Semantic View.

## Comparison Table

| Dimension | Cortex Search | Cortex Analyst |
|---|---|---|
| Primary job | Search and retrieve relevant content | Turn natural-language metric questions into SQL-backed answers |
| Main input | Text chunks, documents, transcripts, logs, metadata columns | Structured data plus semantic model or Semantic View |
| Main output | Ranked search results, snippets, context for RAG, source references | SQL, query result, answer text, clarification, suggestions |
| Best fit | "Find the policy clause", "What do these contracts say?", "Search incident notes" | "Revenue by desk", "Exposure by sector", "Churn by segment" |
| Knowledge shape | Unstructured or semi-structured text | Relational facts, dimensions, metrics, joins, filters |
| Governance focus | Indexed source scope, owner rights, access filters, document-level permissions | Semantic definitions, metric consistency, SQL privileges, query review |
| Freshness concern | Index refresh and document pipeline freshness | Table freshness and semantic model correctness |
| Failure mode | Retrieves irrelevant/stale/overexposed context | Generates SQL that is technically valid but semantically wrong |
| Bank example | Policy Q&A, contract search, research assistant, call transcript search | P&L analysis, exposure analysis, trade count trends, client profitability |
| Consultant shorthand | "Search the evidence." | "Ask governed metrics." |

## Decision Rules

- If the user's question should be answered from documents or text evidence, think **Cortex Search**.
- If the user's question should be answered by aggregating rows and metrics, think **Cortex Analyst**.
- If the business cannot agree on metric definitions, fix the semantic layer before exposing Cortex Analyst broadly.
- If the document set has sensitive content, design source curation and access filtering before indexing it.
- If a workflow needs both document evidence and metric answers, consider **Cortex Agents** as an orchestration layer over Search and Analyst.
- Do not use Cortex Search as a substitute for a semantic metrics layer, and do not use Cortex Analyst as a general document chatbot.

## Common Misreads

- **"Both are natural-language features, so either works."** The input shape and trust model are completely different.
- **"Cortex Search answers from tables."** It can index selected table columns, but its job is retrieval over text-like content, not metric aggregation.
- **"Cortex Analyst searches documents."** Analyst works through SQL and semantic modeling, not document retrieval.
- **"An agent solves the choice."** An agent can route between tools, but the underlying Search and Analyst assets still need correct design.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/37 Cortex Search and RAG]]
- [[01 Snowflake/05 Advanced Analytics and AI/34 Cortex Analyst]]
- [[01 Snowflake/05 Advanced Analytics and AI/38 Cortex Agents and CoWork]]
- [[01 Snowflake/05 Advanced Analytics and AI/39 AI Governance, Guardrails, Observability, and Cost]]
- [[01 Snowflake/04 Data Engineering/31 Unstructured File Pipelines and Document Processing]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake AI and ML Pattern]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Business Users Want Self-Service Analytics but Metrics Are Inconsistent]]

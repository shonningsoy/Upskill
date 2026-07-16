---
tags:
  - note-comparison
---

# Comparison - AI_EXTRACT vs Cortex Search

> AI_EXTRACT turns documents into structured fields; Cortex Search retrieves relevant document context. Consultant lens: extraction creates data products, search supports exploration and Q&A.

## Short Answer

Use **AI_EXTRACT** when the business needs known fields, lists, entities, or tables from documents as structured, reviewable output. Use **Cortex Search** when users need to find relevant passages, ask open-ended questions, or retrieve evidence from a document collection.

## Comparison Table

| Dimension | AI_EXTRACT | Cortex Search |
|---|---|---|
| Primary job | Extract structured information from text or files | Retrieve relevant text/document chunks |
| Main input | A file or text plus a response format/questions | Indexed text chunks and searchable attributes |
| Main output | JSON-like structured fields, lists, or tables, optionally with scores | Ranked results/snippets/context for search or RAG |
| Best fit | Known fields from KYC forms, contracts, invoices, statements, reports | Open-ended search over contracts, policies, transcripts, research, runbooks |
| Question style | "What is the agreement date?" "What is the counterparty name?" | "Find policies about market abuse surveillance." |
| Governance focus | Review thresholds, source traceability, field validation, extraction scores | Indexed scope, access filters, owner rights, prompt injection, citations |
| Persistence pattern | Store approved extracted values in tables | Store/index chunks and metadata; retrieve at query time |
| Failure mode | Extracted value is wrong but looks structured | Retrieves irrelevant or unauthorized context |
| Bank example | KYC field capture, clause extraction, document-to-table pipeline | Compliance evidence search, policy assistant, research Q&A |
| Consultant shorthand | "Turn the document into columns." | "Search the document collection." |

## Decision Rules

- If the downstream system needs columns like `counterparty_name`, `agreement_date`, or `termination_notice_days`, use **AI_EXTRACT**.
- If the user needs to explore, ask, compare, or cite passages across many documents, use **Cortex Search**.
- If extracted fields affect legal, KYC, credit, or compliance decisions, add human review and source traceability.
- If a RAG answer depends on document-level permissions, design Cortex Search with curated sources and filters.
- For document assistants, the two often work together: parse/extract key fields for structured workflows, and index text for search/Q&A.
- Persist reviewed extraction outputs instead of recomputing `AI_EXTRACT` in every dashboard or report.

## Common Misreads

- **"AI_EXTRACT is document search."** It extracts requested values; it is not a retrieval service.
- **"Cortex Search extracts fields into a table."** Search retrieves relevant chunks; structured extraction is a separate step.
- **"Structured output means correct output."** Extracted values still need validation, scores, sampling, or human review.
- **"RAG over documents is enough for operations."** If a workflow needs auditable fields, build an extraction pipeline.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/42 Document and Multimodal AI]]
- [[01 Snowflake/05 Advanced Analytics and AI/37 Cortex Search and RAG]]
- [[01 Snowflake/04 Data Engineering/31 Unstructured File Pipelines and Document Processing]]
- [[01 Snowflake/05 Advanced Analytics and AI/39 AI Governance, Guardrails, Observability, and Cost]]
- [[01 Snowflake/05 Advanced Analytics and AI/33 Cortex AI Functions]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Decisions - Choosing a Snowflake AI and ML Pattern]]

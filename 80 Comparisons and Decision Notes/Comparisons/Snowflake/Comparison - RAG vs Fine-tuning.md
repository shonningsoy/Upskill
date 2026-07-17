---
tags:
  - note-comparison
---

# Comparison - RAG vs Fine-tuning

> RAG supplies external context at inference time; fine-tuning changes model behavior using examples. Consultant lens: do not fine-tune a knowledge problem.

## Short Answer

Use **RAG** when the model needs current, governed, or source-backed knowledge from documents, policies, reports, tickets, transcripts, or tables. Use **fine-tuning** when a stable repeated task still has behavior or format problems after good prompting, structured outputs, and retrieval have been tried.

## Comparison Table

| Dimension | RAG / Cortex Search | Cortex Fine-tuning |
|---|---|---|
| Primary job | Retrieve relevant context before generating an answer | Adapt a supported model to behave better on a stable task |
| Solves | Missing facts, changing knowledge, source grounding | Repeated style, classification, extraction, or response-pattern issues |
| Knowledge location | External documents, chunks, search indexes, tables, semantic sources | Learned from reviewed training examples |
| Best fit | Policies, procedures, research, contracts, runbooks, case notes | Stable output format, repeated task behavior, domain-specific extraction/classification |
| Freshness | Update source data/indexes | Re-train or change model if examples or task definition changes |
| Evidence | Can cite or trace retrieved source content | Does not inherently cite source evidence |
| Governance focus | Source curation, access filters, owner rights, prompt injection, citations | Training data approval, model privileges, evaluation set, lifecycle and rollback |
| Cost surface | Search indexing/serving plus inference/token costs | Training tokens, fine-tuned inference, storage, evaluation |
| Failure mode | Retrieves weak or unauthorized context; answer not grounded | Learns inconsistent examples or becomes brittle outside the training pattern |
| Consultant shorthand | "Give the model the right facts." | "Teach the model the right behavior." |

## Decision Rules

- If the answer depends on changing policies, procedures, contracts, research, or operational knowledge, start with **RAG**.
- If the model answers with the right facts but the wrong structure, wording, label taxonomy, or extraction pattern, evaluate **fine-tuning**.
- If the problem is poor document chunking, missing access filters, or weak source curation, fix the retrieval pipeline before tuning.
- If a task has no stable expected output, avoid fine-tuning until the business has a rubric.
- If a bank needs evidence, citations, or reviewer traceability, RAG/source traceability still matters even when a fine-tuned model is used.
- Treat fine-tuning as a production commitment: it needs test sets, owners, model lifecycle monitoring, and rollback.

## Common Misreads

- **"Fine-tuning teaches the model our bank's latest policies."** Put changing knowledge in governed sources and retrieval, not in model weights.
- **"RAG makes the model behave exactly how we want."** RAG supplies context; it does not guarantee stable formatting or taxonomy behavior.
- **"Fine-tuning removes the need for prompt design."** Fine-tuned models still need clear prompts, schemas, and evaluation.
- **"A small successful demo proves tuning is worth it."** Compare against a strong prompt/RAG baseline with a held-out evaluation set.
- **"RAG and fine-tuning are mutually exclusive."** They can be combined: retrieve facts, then use a tuned model for stable response behavior.

## Related Learning Topics

- [[01 Snowflake/05 Advanced Analytics and AI/37 Cortex Search and RAG]]
- [[01 Snowflake/05 Advanced Analytics and AI/43 Cortex Fine-tuning, Provisioned Throughput, and Model Lifecycle]]
- [[01 Snowflake/05 Advanced Analytics and AI/39 AI Governance, Guardrails, Observability, and Cost]]
- [[01 Snowflake/05 Advanced Analytics and AI/42 Document and Multimodal AI]]
- [[01 Snowflake/05 Advanced Analytics and AI/33 Cortex AI Functions]]

## Related Decision Notes

- [[80 Comparisons and Decision Notes/Decision Notes/Snowflake/Decisions - Choosing a Snowflake AI and ML Pattern]]

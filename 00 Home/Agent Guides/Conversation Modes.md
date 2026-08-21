---
status: active
note_type: agent-guide
last_verified: 2026-08-21
sensitivity: public-generalized
tags:
  - agent-guide
  - knowledge-base
---

# Conversation Modes

These modes are repository conventions for shaping Codex or ChatGPT responses. They
are not product-level modes and do not limit which vault folders or public sources may
be used.

## Mode Selection

The user can explicitly request a mode. Otherwise infer it from intent:

| User intent | Mode |
|---|---|
| Ask, explain, summarize, compare informally, or explore | General |
| Learn a concept, test understanding, or build fluency | Learning |
| Design a solution, evaluate options, or make a technical recommendation | Architecture |
| Scope work, plan delivery, write spikes or stories, or prepare a backlog | Delivery Planning |

Use General when no specialized mode clearly applies. Do not ask which mode to use
unless the answer cannot be inferred and the choice would materially change the work.

Concise questions such as "what is," "how does," or "what is the difference" default
to General unless the user asks to learn. Prompts such as "teach me," "quiz me," "walk
me through," or "help me build fluency" indicate Learning.

Modes can be combined. Say which modes are being combined only when this helps set
expectations. Apply the shared source-routing and reasoning safeguards in `AGENTS.md`
regardless of mode.

## General Mode

Use General mode for normal questions, summaries, explanations, and exploratory
discussion that do not need a full learning, architecture, or delivery framework.

### Behavior

- Lead with the direct answer.
- Use the vault when it materially improves the answer.
- Research public sources when freshness, verification, or a knowledge gap requires it.
- Add structure only where it improves clarity.
- State assumptions only when they materially affect the answer.
- Do not force a decision framework onto a simple question.
- Do not edit the vault unless explicitly asked.

### Typical Shape

1. Direct answer.
2. Essential explanation or comparison.
3. Material caveat, source, or next step if needed.

## Learning Mode

Use Learning mode when the goal is to understand a concept and develop practical
fluency. Default depth is consultant and senior-practitioner working knowledge, not an
exhaustive reference manual.

### Behavior

- Lead with a clear mental model.
- Explain what the concept is, what problem it solves, and where it fits.
- Cover what it can and cannot do.
- Use short examples, readable snippets, or one useful diagram when they add value.
- Connect the subject to cost, performance, governance, security, and operations where relevant.
- Surface common misconceptions and practical failure modes.
- Invite or answer follow-up questions without prematurely writing notes.
- Update durable notes only when explicitly asked.

### Typical Shape

1. Mental model and executive summary.
2. Capabilities and boundaries.
3. How it works.
4. Practical example.
5. Trade-offs and common pitfalls.
6. Consultant or senior-engineer talking points.

Keep conversational teaching shorter and more direct than the corresponding durable
topic note. The note-writing structure is defined in
`00 Home/Agent Guides/Vault Authoring Guide.md`.

## Architecture Mode

Use Architecture mode when the user needs a design, option assessment, trade-off
analysis, or technical recommendation for a generalized scenario.

### Analysis Sequence

1. Restate the goal and system boundary briefly.
2. Extract functional requirements, non-functional requirements, constraints, and unknowns.
3. Separate hard requirements from preferences and assumed constraints.
4. Read relevant factual vault notes.
5. Verify missing, uncertain, or time-sensitive claims with current primary sources.
6. Develop viable options from the present requirements.
7. Compare options using the decision drivers that matter in this case.
8. Form an independent recommendation.
9. Consult reasoning-layer notes as a late challenge for omitted options, risks, or counterarguments.
10. State what evidence or changed condition would alter the recommendation.

Do not retrieve a stored scenario and treat it as the answer. A similar scenario may
suggest questions, but similarity of technology does not establish similarity of
requirements, ownership, risk appetite, or operating model.

### Typical Output

Use only the sections needed, normally drawn from:

1. Short recommendation.
2. Understanding of the situation and boundaries.
3. Requirements and decision drivers.
4. Assumptions and important unknowns.
5. Viable options.
6. Trade-off comparison.
7. Recommended architecture and why.
8. Data quality, security, governance, cost, reliability, and operational implications.
9. Credible alternative and when it becomes preferable.
10. Suggested validation, spike, proof of concept, or next decision.

### Quality Rules

- Label documented facts, user-provided context, assumptions, and architectural judgment.
- Prefer a conditional recommendation when material facts remain unknown.
- Do not invent organization-specific standards or claim final approval.
- Include failure recovery, observability, ownership, and change management where material.
- Test the preferred option against at least one credible alternative.
- Explain which assumptions must hold for the recommendation to remain valid.

## Delivery Planning Mode

Use Delivery Planning mode for technical discovery, Scrum planning, scoping, spikes,
backlog decomposition, sequencing, acceptance criteria, and release planning.

### Analysis Sequence

1. Clarify the outcome and how success will be observed.
2. Define scope, non-scope, assumptions, and unresolved decisions.
3. Identify dependencies, owners, constraints, and non-functional requirements.
4. Separate discovery from implementation.
5. Create spikes for genuine uncertainty that blocks responsible estimation or design.
6. Decompose implementation into testable vertical outcomes where practical.
7. Add validation, observability, rollout, rollback, and operational handover work.
8. Make sequencing and decision gates explicit.

### Typical Output

Use only the sections needed, normally drawn from:

1. Objective and measurable outcome.
2. Scope and non-scope.
3. Assumptions and open questions.
4. Technical approach or required architecture decision.
5. Discovery questions and spikes.
6. Epics, stories, or tasks.
7. Dependencies and proposed sequence.
8. Non-functional requirements and controls.
9. Acceptance criteria.
10. Testing, data reconciliation, and evidence.
11. Observability and operational ownership.
12. Rollout, rollback, recovery, and Definition of Done.
13. Delivery risks and unresolved decisions.

### Quality Rules

- Do not imply that all unknowns must be resolved before useful planning can begin.
- When the user says an architecture or design is fixed, do not reopen it unless a
  contradiction or material risk makes delivery unsafe. Flag that issue explicitly
  instead of silently switching to Architecture mode.
- Label estimates as assumptions unless grounded in team evidence.
- Avoid stories that merely name technical components; connect work to observable outcomes.
- Keep acceptance criteria testable and separate from implementation suggestions.
- Include data correctness, replay, reconciliation, access, and audit evidence when relevant.
- Make external approvals or dependencies visible without inventing their process.

## Combining Architecture and Delivery Planning

When both modes apply, use this order:

```text
Current situation
  -> requirements and decision drivers
  -> architecture options
  -> recommendation or explicit decision gate
  -> validation spikes
  -> delivery scope and sequence
  -> stories, acceptance criteria, rollout, and ownership
```

If the architecture is sufficiently clear, plan against the recommended direction and
state its assumptions. If it is not, plan a bounded discovery phase first. Do not create
a detailed implementation backlog that silently assumes an unresolved architecture.

## Source and Confidence Signals

For substantial answers, make provenance easy to evaluate without turning the response
into an audit report:

- Cite relevant local notes when they influenced factual understanding.
- Cite current public sources for material external claims.
- In Codex Desktop, cite local notes with Markdown links using absolute filesystem paths.
- In saved Obsidian notes, use wikilinks for vault material and Markdown links for public sources.
- Call synthesized recommendations "recommendations" or "judgment," not facts.
- State material uncertainty and the smallest investigation that would reduce it.
- If a reasoning-layer note influenced the analysis, use it for dimensions or challenges,
  not as proof that the same answer applies.

## Saving Results

Conversation output is temporary by default. Save only when the user asks.

When saving:

1. Read `00 Home/Agent Guides/Vault Authoring Guide.md`.
2. Preserve generalized, reusable knowledge rather than the work scenario.
3. Remove sensitive or identifying detail.
4. Distinguish source-backed facts from synthesized guidance.
5. Record assumptions, alternatives, and conditions that change a recommendation.
6. Add source and verification metadata only when it is accurate.

The durable note should read as a clean reference, not a conversation transcript.

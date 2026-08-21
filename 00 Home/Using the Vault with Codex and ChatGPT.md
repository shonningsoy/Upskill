# Using the Vault with Codex and ChatGPT

This vault can act as a conversational knowledge base for learning, technical architecture, and delivery planning. It is not expected to contain every answer. Codex or ChatGPT should combine relevant vault material with current public research and fresh reasoning for the situation you describe.

The vault is a starting point and memory aid, not a closed corpus or an authority that overrides current evidence.

## Four Conversation Modes

These are repository-defined working conventions, not product settings. Name a mode in your prompt when you want predictable output; otherwise the assistant should infer the most suitable mode.

### General mode

Use for direct questions, summaries, explanations, brainstorming, and exploratory discussion.

Expected behavior:

- Answer the actual question without forcing a large framework onto it.
- Use the vault when it materially improves the answer.
- Research externally when the question needs missing, current, or verified information.
- State assumptions only when they could materially change the answer.

### Learning mode

Use when you want to learn a technology or concept.

Expected behavior:

- Lead with a clear mental model.
- Explain what the concept can and cannot do.
- Use short examples and consultant-relevant trade-offs.
- Explore follow-up questions conversationally.
- Do not update notes until you ask to save the durable learning.

### Architecture mode

Use to design, compare, or challenge a technical solution.

Expected behavior:

- Extract requirements, constraints, decision drivers, and unknowns from the current prompt.
- Use relevant factual topic notes and current official documentation.
- Compare credible alternatives rather than matching the question to a stored scenario.
- Recommend a preferred approach and explain the assumptions that make it appropriate.
- Cover security, governance, cost, performance, reliability, recoverability, observability, and operational ownership where relevant.
- Identify questions or experiments that could change the recommendation.

### Delivery Planning mode

Use for Scrum work, technical discovery, scoping, spikes, backlog decomposition, or implementation planning.

Expected behavior:

- State the objective, scope, non-scope, and assumptions.
- Separate discovery spikes from implementation stories when the technical direction is uncertain.
- Identify dependencies, sequencing, non-functional requirements, acceptance criteria, testing, reconciliation, observability, rollout, rollback, recovery, ownership, and delivery risks.
- Base the plan on an architecture decision or explicitly flag the unresolved decision.

Modes can be combined. For example, ask for Architecture plus Delivery Planning when you want both a recommended solution and an actionable backlog. The assistant should frame the architecture first, then build the plan from it.

## How Vault and Web Research Work Together

A substantial answer should normally use sources in this order:

1. Your sanitized situation, technologies, goal, and constraints define the current problem.
2. Relevant topic notes provide reusable knowledge and mental models.
3. Current official documentation verifies changing or material product facts.
4. Reputable primary or secondary public sources fill gaps that official documentation does not cover.
5. The assistant synthesizes a recommendation and labels assumptions and architectural judgment.

The assistant should not browse merely to make an answer look more researched. External research is appropriate when:

- the vault lacks material information;
- the information is time-sensitive or may be outdated;
- the recommendation depends on current product behavior, limitations, compatibility, pricing, licensing, support, or security;
- a material technical claim requires verification;
- vault sources conflict or appear uncertain;
- the question extends materially beyond the vault; or
- you explicitly request external investigation.

When browsing, the assistant should prefer official vendor documentation for product facts, primary sources for standards or research, and reputable secondary sources only when primary material is insufficient.

### Citation style

- In Codex Desktop responses, cite the relevant local Markdown file as a Markdown link using its absolute filesystem path so it is clickable.
- In a ChatGPT Project, cite the exact vault filename or the source citation exposed by the project.
- In saved Obsidian notes, use normal wikilinks for vault material and Markdown links for public sources.
- Place citations close to the material claim they support; do not present a reasoning-layer note as factual evidence.

## How Reasoning Notes Are Used

The material under `80 Comparisons and Decision Notes/` is deliberately useful but not authoritative.

- Comparisons help discover options and trade-off dimensions.
- Decision notes provide diagnostic questions and reusable frameworks.
- Client scenarios are synthetic exercises that can expose risks, alternatives, and overlooked assumptions.

The assistant should not begin by finding the most similar stored scenario or reuse its recommendation as evidence. It should first reason from your current requirements, verify relevant technical facts, develop an independent recommendation, and then use reasoning notes to challenge the result.

This matters because multiple answers can be defensible. A stored scenario may have made different assumptions about scale, latency, recovery, skills, ownership, controls, cost, or deadlines.

## Describing Work Situations Safely

Use generalized, sanitized scenarios. Do not include confidential documents, production data, credentials, personal data, non-public identifiers, internal URLs, sensitive incidents, material non-public information, or details that identify an employer or system.

A useful prompt contains:

- **Situation:** What is happening, described generically.
- **Technology:** Relevant platforms, services, languages, and deployment environment.
- **Goal:** The technical or business outcome.
- **Constraints:** Latency, volume, recovery, security, cost, skills, deadlines, or support boundaries.
- **Question:** The decision, explanation, or plan you need.

If important company-specific information is unavailable, the assistant should proceed with reasonable, explicit assumptions and identify which unknowns could change the result. Generic technical advice does not replace architecture, security, risk, privacy, or regulatory approval.

## Saving Results to the Vault

Conversation does not automatically become vault content.

- The assistant should not edit notes unless you explicitly ask to save, update, summarize into Obsidian, or make repository changes.
- Saved material should be generalized and reusable rather than a record of the work situation.
- Durable notes should retain mental models, decision rules, alternatives, assumptions, examples, failure modes, and verified facts.
- Raw dialogue, company details, temporary task context, and weak or superseded reasoning should be omitted.
- If current public documentation conflicts with a vault note, the assistant should disclose the conflict and offer to update the affected note.

Useful save requests include:

```text
Summarize the durable learning from this discussion into the relevant topic note.
```

```text
Create a reusable decision framework from this discussion. Remove all situational details and make alternative valid recommendations explicit.
```

## Example Prompts

### General

```text
Use general mode. Explain the practical difference between idempotency and deduplication in a data pipeline. Use the vault where relevant and research externally only if needed.
```

### Learning

```text
Use learning mode. Teach me how API pagination and incremental state interact in a production ingestion pipeline. Start with the mental model, then give a small example and the main operational pitfalls. Do not update the vault yet.
```

### Architecture

```text
Use architecture mode.

Situation: A batch pipeline retrieves transactions from a paginated API and loads Snowflake. Late corrections can arrive for seven days, and consumers need data within 30 minutes.

Technology: Python, Docker, Airflow, Snowflake, and dbt Core.

Goal: Recommend an ingestion and transformation design that supports replay, reconciliation, observability, and controlled cost.

Constraints: Moderate volume, a small support team, at-least-once delivery from the source, and no sensitive company information in this prompt.

Use relevant vault content but do not limit the investigation to it. Verify material current product behavior using official public sources. Separate documented facts, assumptions, and architectural judgment. Compare credible alternatives and identify what could change the recommendation.
```

### Architecture plus Delivery Planning

```text
Use architecture and delivery-planning modes. Recommend a design for the sanitized scenario below, then translate the recommendation into discovery questions, technical spikes, implementation stories, dependencies, non-functional requirements, acceptance criteria, testing and reconciliation, observability, rollout, rollback, ownership, and major delivery risks.

[Insert situation, technology, goal, and constraints.]
```

### Challenge an existing answer

```text
Use architecture mode. Build the recommendation independently from the current requirements. Use notes under 80 Comparisons and Decision Notes only to discover alternatives, test assumptions, and challenge your conclusion. Do not treat a stored scenario as evidence.
```

## Practical Expectation

The best answers will sometimes be conditional. It is valid for the assistant to say that one option is preferable under stated assumptions while identifying a second option that becomes better if latency, operating ownership, recovery objectives, cost, or security constraints change.

That is the intended behavior: reason from the current situation, use the vault as durable context, investigate public gaps, and make uncertainty visible.

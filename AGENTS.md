# AGENTS.md

## Purpose

This repository is an Obsidian vault for two related purposes:

1. Structured learning about the modern data stack.
2. A chat-based knowledge and reasoning aid for generalized data-engineering work.

The learner is a senior data engineer and consultant who also contributes to Scrum,
technical discovery, scoping, and solution design. Optimize answers for practical
working knowledge, architectural judgment, and delivery usefulness rather than
exhaustive product documentation.

The vault is a preferred source, not a closed corpus or the sole authority. Combine
relevant vault knowledge with current public sources and independent reasoning when
the question requires it.

## Repository

Local vault path:

```text
C:/Prosjekter/Upskill
```

GitHub repository:

```text
https://github.com/shonningsoy/Upskill
```

Use the existing top-level structure:

```text
00 Home/
01 Snowflake/
02 dbt/
03 Fivetran/
04 Docker/
05 APIs/
80 Comparisons and Decision Notes/
90 Templates/
99 Archive/
```

Do not reorganize these folders unless explicitly asked.

## Required Guides

- For mode behavior and answer patterns, read
  `00 Home/Agent Guides/Conversation Modes.md` when the request involves learning,
  architecture, technical discovery, scoping, or delivery planning.
- Before creating or changing durable vault content, read
  `00 Home/Agent Guides/Vault Authoring Guide.md` completely.

## Conversation Modes

Use one or more of these modes:

- **General:** Direct answers, summaries, explanations, and exploratory discussion.
- **Learning:** Conversational teaching, mental models, examples, and knowledge checks.
- **Architecture:** Technical solution design, option evaluation, and recommendations.
- **Delivery Planning:** Discovery, scoping, spikes, backlog construction, and rollout planning.

The user may name a mode explicitly. Otherwise infer it from the request and use
General when no specialized mode clearly applies. Do not ask the user to choose a
mode unless the choice would materially change the work and cannot be inferred.

Modes may be combined. For Architecture plus Delivery Planning, first establish or
frame the technical direction and unresolved decisions, then translate that result
into delivery work. Do not disguise architectural uncertainty inside implementation
stories; create a decision or spike where appropriate.

## Source Routing

For substantive questions:

1. Understand the current situation, goal, constraints, and unknowns from the prompt.
2. Decompose the question into factual, contextual, and decision-oriented parts.
3. Search relevant factual topic notes in the vault.
4. Identify gaps, uncertain claims, and facts that may have changed.
5. Verify material current claims with public primary sources when needed.
6. Use reasoning notes only as safeguards, prompts, and counterarguments.
7. Synthesize the answer independently for the current situation.
8. Distinguish documented facts, user-provided context, assumptions, and judgment.
9. Cite the local notes and external sources that materially support the answer.

In Codex Desktop responses, cite a local vault file as a Markdown link using its
absolute filesystem path. In saved Obsidian notes, use wikilinks for vault material
and Markdown links for public sources. Place citations close to the claims they support.

The user's prompt defines the case but does not establish general product facts. Prefer
current official vendor documentation for product behavior, then primary standards or
research, then reputable secondary sources where primary sources are insufficient. The
final recommendation is a synthesis and must be presented as architectural or delivery
judgment.

Treat notes marked `status: seed`, `status: draft`, `status: reference`, or
`validation_status: unvalidated` as leads, not evidence. Prefer active current notes
over archived or reference summaries. If current official sources conflict with the
vault, prefer the current official sources, disclose the conflict, and offer to update
the affected note.
For material volatile product claims, a note without a trustworthy `last_verified`
date or source date is unverified; check current official documentation.

## When to Use Web Research

Do not browse merely to make an answer appear more researched.

Browse when:

- the vault lacks information material to the answer;
- information may be outdated or time-sensitive;
- the recommendation depends on current product behavior, limitations,
  compatibility, pricing, licensing, support, or security;
- a material technical claim requires verification;
- vault sources conflict or appear uncertain;
- the question extends materially beyond the vault; or
- the user explicitly requests external investigation.

When browsing:

- prefer current official documentation for product facts;
- prefer primary sources for standards and technical research;
- use secondary sources only when primary sources are insufficient;
- cite sources close to the claims they support;
- separate externally verified facts from architectural inference; and
- avoid re-researching stable facts already supported by reliable, current material.

If browsing is unavailable, state which material claims could not be verified.

## Reasoning-Layer Safeguards

Content under `80 Comparisons and Decision Notes/` is a reasoning aid, not an
authoritative answer source or collection of production recommendations.

- Do not start architecture analysis by choosing a superficially similar scenario.
- Do not use a scenario's outcome as evidence for the current question.
- Do not assume matching technologies imply matching requirements.
- Derive requirements and decision drivers from the current prompt first.
- Use comparison notes to discover options and trade-off dimensions.
- Use decision notes as diagnostic frameworks and question checklists.
- Use client scenarios as synthetic exercises, counterexamples, or hypothesis generators.
- Verify technical claims with factual notes and current primary sources as needed.
- Build the recommendation independently for the present situation.
- State the assumptions required for reused reasoning to apply.
- Consider at least one credible alternative to a preferred recommendation.
- Use reasoning notes late in analysis to challenge omissions and anchoring.

Topic notes provide reusable knowledge. Public sources provide breadth, verification,
and freshness. Reasoning notes provide prompts and counterarguments. The current
situation determines the recommendation.

## Privacy and Sanitization

Assume work questions are intentionally generalized and contain no sensitive company
information. Do not try to identify the employer, internal systems, or people involved.

- Do not request internal documentation unless the user explicitly changes the scope.
- Do not invent firm-specific policies, approvals, controls, or infrastructure.
- Treat missing organization-specific facts as labelled assumptions or decision questions.
- Do not save scenario details to the vault unless the user explicitly asks.
- When saving, generalize reusable knowledge and remove client names, system names,
  internal identifiers, URLs, incident details, schemas, credentials, and non-public controls.
- Never store production data, secrets, personal data, material non-public information,
  or confidential internal documents in this repository.
- State when a recommendation still requires architecture, security, risk, privacy,
  regulatory, or operational approval.

## Read and Write Behavior

Answering, teaching, reviewing, investigating, or planning does not by itself authorize
vault edits. Search and read freely, but update files only when the user explicitly asks
to save, update, create, implement, or otherwise change durable content.

When the user is exploring a subject, prioritize the conversation. When the user says
"update the note," "save this," "summarize into Obsidian," or similar, distill only the
material likely to remain useful. Do not turn raw conversation into a transcript.

Before any durable edit, read the authoring guide, inspect relevant notes and templates,
check `git status --short --branch`, preserve unrelated user changes, and follow the
existing folder structure and Obsidian wikilink conventions.

Use forward slashes in Obsidian wikilinks, for example:

```md
[[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
```

Do not remove existing frontmatter unless explicitly asked. Do not create comparison,
decision, or scenario notes merely to mirror the curriculum; create them only when a
discussion produces durable reasoning material.

## Durable Notes

The vault should retain distilled understanding: mental models, verified facts,
decision drivers, trade-offs, examples, pitfalls, and consultant talking points.

When external investigation produces durable value, offer to update the relevant note.
An update should preserve the distinction between verified facts and synthesized
guidance, include useful sources, and record verification metadata only after actual
verification.

## Git Workflow

Before editing, run `git status --short --branch`. After meaningful authorized changes,
commit and push only when the user asks or when the active task explicitly includes
that workflow:

```bash
git add <specific-files>
git commit -m "Describe the knowledge-base update"
git push
```

Stage specific files rather than `git add .` when unrelated changes exist. Never commit
Obsidian local workspace state. Preserve user-owned `.obsidian/graph.json` and
`workspace.json` changes unless explicitly asked to modify them.

## Core Preference

Keep answers direct, evidence-aware, and useful for senior data-engineering decisions.
Keep the vault familiar and consistent. Prefer stable structure and explicit reasoning
over clever formatting, false certainty, or retrieval of a predetermined answer.

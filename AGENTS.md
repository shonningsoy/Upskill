# AGENTS.md

## Purpose

This repository is an Obsidian vault for structured upskilling in the modern data stack, starting with Snowflake and later expanding into dbt and Fivetran.

The learner is preparing for a consultant role. The goal is not to deep-dive into every technical detail immediately, but to build a strong "title and subtitle" understanding of tools, features, trade-offs, and practical use cases.

The vault should help the learner answer questions like:

- What is this feature?
- What problem does it solve?
- What can it do?
- What can it not do?
- When would I recommend it to a client?
- What are the risks, trade-offs, cost implications, or governance concerns?
- What small code/config snippets help me recognize it in practice?

## Repository Location

Local vault path:

```text
D:\Shonningsoy
```

GitHub repository:

```text
https://github.com/shonningsoy/Upskill
```

## Main Structure

Use the existing folder structure.

```text
00 Home/
01 Snowflake/
02 dbt/
03 Fivetran/
80 Comparisons and Decision Notes/
90 Templates/
99 Archive/
```

Do not reorganize folders unless explicitly asked.

The `80 Comparisons and Decision Notes/` area is the consultant reasoning layer. Use these subfolders:

```text
80 Comparisons and Decision Notes/Comparisons/
80 Comparisons and Decision Notes/Decision Notes/
80 Comparisons and Decision Notes/Client Scenarios/
```

- `Comparisons/` is for A vs B trade-off notes.
- `Decision Notes/` is for broader recommendation frameworks.
- `Client Scenarios/` is for realistic client problem statements and reasoning paths.
- Keep `80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview.md` updated as the hub for this area.

## Current Learning Focus

The current focus is Snowflake.

The main navigation note is:

```text
00 Home/Snowflake Learning Map.md
```

Snowflake is organized into chapter folders:

```text
01 Snowflake/01 Core Architecture and Concepts/
01 Snowflake/02 Performance and Optimization/
01 Snowflake/03 Security and Governance/
01 Snowflake/04 Data Engineering/
01 Snowflake/05 Advanced Analytics and AI/
01 Snowflake/06 Cost Management and Operations/
01 Snowflake/07 Ecosystem and Integration/
```

Each chapter has an overview note. Topic notes should link back to the relevant overview note and to a small number of genuinely related topics.

## Note Style

Use the existing template:

```text
90 Templates/Topic Note Template.md
```

Each topic note should generally include:

- Executive Summary
- What It Can Do
- What It Cannot Do
- Core Concepts
- How It Works
- Readable Snippets
- Consultant Talking Points
- Common Pitfalls
- Related Topics
- Questions
- Sources To Revisit

Topic notes may also include:

- Related Decision and Scenario Notes

Keep notes practical, scannable, and consultant-oriented.

Avoid turning notes into documentation dumps. Prefer concise explanations, comparison tables, small examples, and clear decision rules.


## Topic Note Consistency Rules

To keep the vault familiar across Snowflake, dbt, and Fivetran, use one stable note structure:

- Keep the section order defined in `90 Templates/Topic Note Template.md`.
- Write `How It Works` as a simple numbered flow (typically 4-8 steps).
- Include practical `Common Pitfalls` tied to cost, performance, governance, or operations.
- Include a meaningful `When to Recommend What (Decision Table)` for notes that move beyond seed placeholders.
- After a topic note is filled out, add 1-3 curated links to relevant comparison, decision, or client scenario notes when they exist.
- When a topic creates a useful new client situation or recommendation trade-off, add or update a note under `80 Comparisons and Decision Notes/`.

## Depth Target

Default depth is "consultant working knowledge," not exhaustive implementation depth.

Good:

- Clear mental models
- When to use / when not to use
- Common client questions
- Governance, cost, performance, and operational implications
- Short SQL/config snippets that are easy to read

Avoid by default:

- Large demo projects
- Long code monoliths
- Exhaustive syntax references
- Deep internals unless the learner explicitly asks

## Obsidian Linking Rules

Use normal Obsidian wikilinks.

Always use forward slashes in links:

```md
[[01 Snowflake/04 Data Engineering/19 Dynamic Tables]]
```

Do not use Windows backslashes inside wikilinks:

```md
[[01 Snowflake\04 Data Engineering\19 Dynamic Tables]]
```

Bad links can create duplicate blank notes in Obsidian Graph View.

## Tags

Snowflake notes use chapter tags for Graph View coloring.

Use these tags consistently:

```text
sf-core-architecture
sf-performance
sf-security-governance
sf-data-engineering
sf-analytics-ai
sf-cost-ops
sf-ecosystem-integration
```

Do not remove existing frontmatter unless explicitly asked.

## Graph View

The graph should remain simple.

Preferred setup:

- Snowflake Learning Map links to chapter overview notes.
- Chapter overview notes link to topic notes.
- Comparisons and Decision Notes Overview links to comparison, decision, and scenario notes.
- Scenario/decision notes link to the few topic notes needed to reason through the situation.
- Topic notes link to their overview note and selected related topics.
- Use tags for Graph View color groups.

Avoid file-based Graph View hacks or excessive cross-linking. Too many links make the graph noisy. A comparison or scenario note should usually link to 3-6 genuinely relevant topic notes, not every vaguely related note.

## Durable Notes Principle

The vault should contain distilled understanding, not raw conversation. Notes should preserve what will still be useful weeks later: mental models, decision rules, examples, trade-offs, pitfalls, and consultant talking points.

## Collaboration Pattern

When helping the learner with a topic, the agent should treat the conversation as a learning dialogue first and a note-writing task second.

The learner may ask follow-up questions, test understanding, challenge explanations, or temporarily explore side paths. Do not update the vault after every message unless the learner explicitly asks for that. Some parts of the conversation may be exploratory, repetitive, mistaken, or ultimately not useful.

Preferred workflow:

1. Teach the concept conversationally.
2. Answer follow-up questions and help the learner reason through the topic.
3. Keep track of useful insights, distinctions, examples, and questions that emerge.
4. When the discussion reaches a natural stopping point, summarize what was learned.
5. Weed out tangents, weak explanations, and non-useful details.
6. Update the relevant Obsidian note with only the durable material worth keeping.
7. Add useful related links and short readable snippets where helpful.
8. Add or update comparison, decision, or client scenario notes if the topic produced durable "when would I recommend this?" material.
9. Commit and push if the user asks, or if the session clearly produced durable vault updates.

The final note should read like a clean consultant reference, not like a transcript of the conversation.

If the learner says something like "update the note," "save this," "summarize into Obsidian," or "this is worth keeping," then update the relevant file immediately.

If the learner is still exploring, prefer conversation over file edits.

## Git Workflow

Before making edits, check status:

```bash
git status --short --branch
```

After meaningful changes:

```bash
git add .
git commit -m "Describe the learning note update"
git push
```

Do not commit Obsidian local workspace state. `workspace.json` is intentionally ignored.

## Visuals Policy

Use visuals to improve consultant learning speed where relevant.

- Prefer embedded local images in notes (store files under `00 Home/assets/`).
- Default target is one useful visual per topic note when relevant and possible.
- A topic note may omit a visual only if no high-value visual is available; in that case, add a one-line note in `Visuals` stating this explicitly.
- Prioritize official sources (Snowflake, dbt, Fivetran).
- If official visuals are not available, use reputable sources.
- Keep source links in `Sources To Revisit` for each note using external visuals.

## Important Preference

The vault should feel familiar and consistent from note to note. Prioritize a stable structure over clever formatting.

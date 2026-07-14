# Obsidian Workflow for Learning

Obsidian is most useful here as a thinking system, not a document archive. The goal is to build a personal consulting knowledge base that is easy to scan before client discussions, interviews, workshops, and implementation work.

## The basic idea

Each learning topic should become one durable note. A durable note is not a transcript of everything you learned. It is a reusable explanation you can come back to later.

For this vault, each topic note should answer five questions:

1. What is it?
2. What problem does it solve?
3. What can it do?
4. What can it not do?
5. What would I say to a client about it?

## Daily workflow

1. Open [[Snowflake Learning Map]].
2. Pick the next unfinished topic.
3. Ask Codex to teach the topic at consultant level.
4. Ask Codex to update the topic note in the standard format.
5. Add any unclear points to [[Questions Inbox]].
6. Link related notes as they appear.

## How to work with Codex

Useful prompts:

- "Teach me `Virtual Warehouses` at title-and-subtitle level for a consultant role. Update the note in my Obsidian vault."
- "Add readable Snowflake SQL snippets to this note, but keep them short and explanatory."
- "Compare `Dynamic Tables` and `Streams and Tasks` and create a decision note."
- "Review this note and tell me what is unclear, inaccurate, or too deep for my current goal."
- "Make this note easier to scan before a client meeting."

## Linking rules

Use links when two topics help explain each other:

- Link features that are alternatives.
- Link features that are often used together.
- Link concepts that explain the same client decision.
- Link Snowflake notes to future dbt and Fivetran notes when the modern data stack picture becomes relevant.

Examples:

- [[01 Snowflake/01 Core Architecture and Concepts/01 Virtual Warehouses]] relates to [[01 Snowflake/06 Cost Management and Operations/44 Credit Consumption Model]].
- [[01 Snowflake/04 Data Engineering/21 Dynamic Tables]] relates to [[01 Snowflake/04 Data Engineering/20 Streams and Tasks]].
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]] relates to [[01 Snowflake/03 Security and Governance/15 Data Classification]].

## Tags

Tags should be few and boring. Use folders and links for structure. Use tags only for cross-cutting concerns.

Recommended tags:

- `#snowflake`
- `#dbt`
- `#fivetran`
- `#security`
- `#performance`
- `#cost`
- `#governance`
- `#data-engineering`
- `#consulting`

## When a note is done enough

A note is useful when you can answer these without opening documentation:

- What is this feature?
- When would I recommend it?
- What are the trade-offs?
- What are the common misunderstandings?
- What snippet or example shows the basic shape?

That is enough for your current goal. You can always deepen a note later when real project work demands it.

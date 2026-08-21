# Start Here

This vault is for consultant-oriented learning and generalized work advice: enough depth to discuss tools clearly, spot fit/non-fit situations, reason about architecture, and turn a technical direction into deliverable work.

For guidance on using the vault as a chat-based knowledge base, source routing, and safe handling of generalized work situations, start with [[00 Home/Using the Vault with Codex and ChatGPT]].

## Conversation modes

Codex and ChatGPT can infer a mode from the question, or you can name one explicitly:

- **General:** direct answers, summaries, and exploratory discussion.
- **Learning:** mental models, capabilities, limitations, examples, and consultant trade-offs.
- **Architecture:** requirements, options, current evidence, recommendation, risks, and assumptions.
- **Delivery Planning:** scope, spikes, stories, dependencies, acceptance criteria, rollout, and operational ownership.

Modes can be combined. For example: `Use Architecture and Delivery Planning modes. Recommend a design, then turn it into spikes and implementation stories.`

## How to use this vault

1. Start from the learning map for the current tool: [[Snowflake Learning Map]], [[02 dbt/dbt Learning Map|dbt Learning Map]], [[03 Fivetran/Fivetran Learning Map|Fivetran Learning Map]], [[04 Docker/Docker Learning Map|Docker Learning Map]], or [[05 APIs/API Learning Map|API Learning Map]].
2. Open the next numbered topic note.
3. During a Codex session, ask for a guided explanation of that topic.
4. After the conversation produces durable understanding, let Codex update the note using [[Topic Note Template]], [[Fivetran Topic Note Template]], or [[Docker Topic Note Template]]. For chat or draft work-oriented outputs, use [[Architecture Decision Template]], [[Technical Spike Template]], or [[Delivery Plan Template]]; these are not saved automatically.
5. Add open questions to [[Questions Inbox]] instead of interrupting the main note.
6. Revisit [[Glossary]] whenever a term appears across several topics.

## Learning rhythm

- First pass: understand the concept, where it fits, and its limits.
- Second pass: read small SQL/config snippets and explain them back.
- Third pass: compare it to neighboring tools or features.
- Consultant pass: write a short "when I would recommend this" answer.

## Folder logic

- `00 Home`: navigation, system notes, glossary, and questions.
- `01 Snowflake`: your Snowflake curriculum and topic notes.
- `02 dbt`: dbt learning map, chapter hubs, and topic notes.
- `03 Fivetran`: Fivetran learning map, chapter hubs, and topic notes.
- `04 Docker`: Docker learning map, chapter hubs, and data-engineering-focused topic notes.
- `05 APIs`: API literacy, data-pipeline consumption, API building, and Snowflake-centered production decisions.
- `80 Comparisons and Decision Notes`: synthesized comparisons, heuristic decision frameworks, and synthetic scenarios used to challenge reasoning—not as sole evidence or ready-made recommendations.
- `90 Templates`: reusable note formats for future topics.
- `99 Archive`: retired notes, old drafts, and replaced material.

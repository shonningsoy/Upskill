# AGENTS.md

## Purpose

This repository is an Obsidian vault for structured upskilling in the modern data stack, starting with Snowflake and expanding into dbt, Fivetran, Docker, and APIs for data-engineering environments.

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
04 Docker/
05 APIs/
80 Comparisons and Decision Notes/
90 Templates/
99 Archive/
```

Do not reorganize folders unless explicitly asked.

The `80 Comparisons and Decision Notes/` area is the consultant reasoning layer. Organize it by note type first, then domain:

```text
80 Comparisons and Decision Notes/Comparisons/Snowflake/
80 Comparisons and Decision Notes/Comparisons/dbt/
80 Comparisons and Decision Notes/Comparisons/Fivetran/
80 Comparisons and Decision Notes/Comparisons/Docker/
80 Comparisons and Decision Notes/Comparisons/Cross-Tool/
80 Comparisons and Decision Notes/Decision Notes/Snowflake/
80 Comparisons and Decision Notes/Decision Notes/dbt/
80 Comparisons and Decision Notes/Decision Notes/Fivetran/
80 Comparisons and Decision Notes/Decision Notes/Docker/
80 Comparisons and Decision Notes/Decision Notes/Cross-Tool/
80 Comparisons and Decision Notes/Client Scenarios/Snowflake/
80 Comparisons and Decision Notes/Client Scenarios/dbt/
80 Comparisons and Decision Notes/Client Scenarios/Fivetran/
80 Comparisons and Decision Notes/Client Scenarios/Docker/
80 Comparisons and Decision Notes/Client Scenarios/Cross-Tool/
```

- `Comparisons/` is for A vs B trade-off notes.
- `Decision Notes/` is for broader recommendation frameworks.
- `Client Scenarios/` is for realistic client problem statements and reasoning paths.
- Use `Snowflake/`, `dbt/`, `Fivetran/`, `Docker/`, and `Cross-Tool/` domain subfolders under each note type. Create the domain folder when it has notes worth keeping.
- Keep `80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview.md` updated as the hub for this area.
- Name comparison files and H1 titles as `Comparison - <topic>`.
- Name decision-note files and H1 titles as `Decisions - <topic>`.
- Name client-scenario files and H1 titles as `Scenario - <client problem>`.
- Use `note-comparison` for comparison notes, `note-decision` for decision notes, and `note-scenario` for scenario notes so Obsidian Graph View can color them.
- Keep one shared consultant reasoning layer for Snowflake, dbt, Fivetran, Docker, and cross-tool decisions. Do not create separate top-level reasoning areas under the individual tool folders.
- Keep the overview note grouped with headings for Snowflake, dbt, Fivetran, Docker, and Cross-Tool notes under each note type.
- Add dbt or Docker decision, comparison, and scenario notes only when a topic discussion produces durable recommendation material. Do not pre-create decision notes just to mirror a topic curriculum.

## Current Learning Focus

The current focus includes Docker and APIs for a Snowflake, dbt Core, Airflow, and Python data-engineering environment, alongside the existing Snowflake, dbt, and Fivetran paths.

The main navigation notes are:

```text
00 Home/Snowflake Learning Map.md
02 dbt/dbt Learning Map.md
03 Fivetran/Fivetran Learning Map.md
04 Docker/Docker Learning Map.md
05 APIs/API Learning Map.md
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
01 Snowflake/08 Enterprise Snowflake in Production/
```

dbt should use the same learning-map and chapter-overview pattern:

```text
02 dbt/01 Core Concepts and Project Structure/
02 dbt/02 Modeling Patterns and Layering/
02 dbt/03 Testing Documentation and Data Quality/
02 dbt/04 Incremental Processing and Performance/
02 dbt/05 Deployment CI CD and Operations/
02 dbt/06 Governance Semantic Layer and Mesh/
02 dbt/07 Packages Macros and Advanced Reuse/
02 dbt/08 dbt on Snowflake and Finance Patterns/
```

Fivetran uses the same learning-map and chapter-overview pattern:

```text
03 Fivetran/01 Foundations and Platform Mental Model/
03 Fivetran/02 Connectors and Sync Behavior/
03 Fivetran/03 Destination Data History and Schema Change/
03 Fivetran/04 Fivetran with Snowflake and dbt/
03 Fivetran/05 Security Governance and Production Operations/
03 Fivetran/06 Cost and Consultant Decision-Making/
```

Docker uses the same learning-map and chapter-overview pattern:

```text
04 Docker/01 Foundations and Everyday Docker/
04 Docker/02 Reproducible Images and Docker Compose/
04 Docker/03 Data Stack Integration Patterns/
04 Docker/04 Security Operations and Team Standards/
```

APIs use the same learning-map and chapter-overview pattern:

```text
05 APIs/01 Foundations and API Literacy/
05 APIs/02 Consuming APIs for Data Pipelines/
05 APIs/03 Designing and Building APIs/
05 APIs/04 Production Security and Data Stack Integration/
```

Each chapter has an overview note. Topic notes should link back to the relevant overview note and to a small number of genuinely related topics.

For dbt, keep the notes focused on consultant working knowledge: what the dbt concept does, how it changes analytics engineering workflow, when to recommend it, and what governance, operational, cost, or banking-control implications matter.

For Fivetran, keep the notes focused on consultant working knowledge: source and connector fit, sync and destination behavior, schema and data-quality boundaries, security and operations, usage cost, and the ownership boundary with Snowflake and dbt. Do not force code into Fivetran notes; use short configuration, destination-row, API, Terraform, SQL, calculation, or checklist examples only when they improve recognition.

For Docker, keep the compact curriculum focused on working fluency in a Snowflake, dbt Core, Airflow, and Python environment: reproducible images, everyday container operation, Docker Compose, dependency and secret boundaries, debugging, security, CI promotion, and production-fit judgment. Preserve the dedicated Data Stack Integration Patterns chapter. Prefer merging related mechanics into the existing 18 topics rather than adding narrow new topics. Snowflake normally remains an external managed service. Do not expand into Kubernetes or deep container internals unless the learner explicitly asks.

For APIs, teach conversation fluency first, then consuming APIs, building with FastAPI/Pydantic, and production judgment. Keep the focus on data-engineering concerns: pagination, incremental state, rate limits, retries, idempotency, schema drift, reconciliation, service identity, Snowflake data access, observability, Docker deployment, and API-versus-file/CDC/connector decisions. Do not turn the curriculum into front-end development, deep protocol internals, or a framework reference.

## dbt Package and Advanced Topic Guidance

Treat dbt packages as tools to evaluate, not as things to install by default.

Default package learning priorities:

- Must understand: `dbt_utils`, `codegen`, `audit_helper`, `dbt_project_evaluator`, and `dbt_expectations`.
- Strong situational value: `elementary`, `dbt_artifacts`, `dbt_date`, `dbt_external_tables`, Snowflake query-tag packages, `dbt_snowflake_monitoring`, and `dbt_constraints`.
- Finance or banking relevance: audit and reconciliation packages, observability packages, Data Vault packages such as `automate_dv` or `datavault4dbt` when the client's modeling strategy uses Data Vault, constraint packages, metadata-testing packages, and Snowflake cost/query-attribution packages.
- Use caution with source-specific, vendor-specific, lightly maintained, or business-logic-heavy packages. In regulated environments, discuss maintenance, license, security review, version pinning, Fusion compatibility, Snowflake compatibility, and support ownership.

Advanced dbt topics that deserve explicit notes include Jinja fundamentals, macros, custom generic tests, adapter dispatch, hooks, operations, package governance, project dependencies, model access, model contracts, model versions, dbt Mesh, semantic models, dbt State, and CI deferral.

## Note Style

Use the existing template:

```text
90 Templates/Topic Note Template.md
```

For Fivetran topics, use the tailored template:

```text
90 Templates/Fivetran Topic Note Template.md
```

For Docker topics, use the tailored template:

```text
90 Templates/Docker Topic Note Template.md
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

- Related Decision Notes

Keep notes practical, scannable, and consultant-oriented.

Avoid turning notes into documentation dumps. Prefer concise explanations, comparison tables, small examples, and clear decision rules.


## Topic Note Consistency Rules

To keep the vault familiar across Snowflake, dbt, Fivetran, Docker, and APIs, use one stable note structure:

- Keep the section order defined in `90 Templates/Topic Note Template.md`.
- Write `How It Works` as a simple numbered flow (typically 4-8 steps).
- Include practical `Common Pitfalls` tied to cost, performance, governance, or operations.
- Include a meaningful `When to Recommend What (Decision Table)` for notes that move beyond seed placeholders.
- After a topic note is filled out, add 1-3 curated links to relevant comparison or decision notes when they exist.
- Prefer not to link topic notes directly to scenario notes unless the scenario is exceptionally central; let scenario notes link to topics and rely on backlinks.
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
[[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
```

Do not use Windows backslashes inside wikilinks:

```md
[[01 Snowflake\04 Data Engineering\21 Dynamic Tables]]
```

Bad links can create duplicate blank notes in Obsidian Graph View.

## Snowflake and dbt Linking Rules

Keep Snowflake and dbt learning areas distinct, but do not make them isolated.

Default rule:

- dbt topic notes should primarily link to their dbt chapter overview and closely related dbt topics.
- Add Snowflake links only when Snowflake materially changes the recommendation, implementation, or risk discussion.
- Useful Snowflake cross-links usually involve warehouses and cost, RBAC, schemas and databases, Dynamic Tables, Streams and Tasks, Snowflake Tasks, query history, Account Usage, masking and row access policies, object tags, data loading, or dbt Projects on Snowflake.
- Prefer linking from dbt chapter overviews to a curated set of Snowflake topics instead of adding many Snowflake links to every dbt topic note.
- Keep `01 Snowflake/04 Data Engineering/25 dbt on Snowflake.md` as the main Snowflake-side bridge into dbt.
- Do not link Snowflake and dbt topics merely because both belong to the modern data stack. Link only when the relationship helps explain a trade-off, operational decision, or implementation pattern.

When a Snowflake topic becomes relevant during a dbt learning conversation, add the link when updating the durable note. Do not force cross-links during exploratory conversation.

## Fivetran Linking Rules

Keep the Fivetran learning area distinct while preserving the practical bridge to Snowflake and dbt.

- Fivetran topic notes should primarily link to their Fivetran chapter overview and closely related Fivetran topics.
- Add Snowflake links when destination identity, RBAC, networking, schemas, warehouses, table types, loading cost, monitoring, or recovery changes the recommendation.
- Add dbt links when source freshness, staging, tests, reconciliation, orchestration, or downstream contracts change the recommendation.
- Prefer curated cross-tool links from the `Fivetran with Snowflake and dbt` chapter overview instead of repeating them throughout every topic.
- Keep `02 dbt/08 dbt on Snowflake and Finance Patterns/81 Fivetran to Snowflake to dbt Flow.md` as the dbt-side bridge into Fivetran.
- Do not link tools merely because they appear in the same stack. Link only when the relationship explains ownership, risk, cost, or an implementation trade-off.

## Docker Linking Rules

Keep the Docker learning area distinct while linking it to the runtime decisions it materially affects.

- Docker topic notes should primarily link to their Docker chapter overview and closely related Docker topics.
- Add Snowflake links when authentication, networking, certificates, proxies, service roles, query attribution, or external-service boundaries change the recommendation.
- Add dbt links when container execution affects dependencies, profiles, artifacts, CI, orchestration, or production jobs.
- Add Airflow context inside Docker notes when Compose services, custom images, providers, task isolation, logs, or executor boundaries matter.
- Prefer curated cross-tool links from `Data Stack Integration Patterns Overview` rather than repeating the whole stack in every Docker topic.
- Do not imply that Docker Compose alone makes an Airflow deployment production-ready.
- Do not link tools merely because they share an environment. Link only when Docker changes reproducibility, execution ownership, security, cost, or operations.

## API Linking Rules

Keep the API learning area cross-tool but curated.

- API topic notes should primarily link to their API chapter overview and closely related API topics.
- Add Snowflake links when service identity, SQL execution, REST resource management, external access, SPCS, workload isolation, query attribution, latency, or cost changes the recommendation.
- Add Fivetran links when managed API connectors, source coverage, sync behavior, schema handling, or build-versus-buy ownership matters.
- Add Docker links when API packaging, secrets, networking, health, CI promotion, or runtime operations matter.
- Add dbt links when curated serving models, contracts, tests, freshness, or precomputation materially shape the API.
- Keep API comparisons and decision notes under the existing `Cross-Tool/` reasoning folders.
- Do not imply that exposing a Snowflake query through HTTP makes it an operationally suitable API.

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
sf-enterprise-production
```

dbt notes use these chapter tags consistently:

```text
dbt-core-projects
dbt-modeling
dbt-quality-docs
dbt-performance
dbt-deployment-ops
dbt-governance-mesh
dbt-packages-macros
dbt-snowflake-finance
```

Fivetran notes use these chapter tags consistently:

```text
fivetran-foundations
fivetran-connectors-sync
fivetran-data-schema
fivetran-snowflake-dbt
fivetran-security-operations
fivetran-cost-decisions
```

Docker notes use these chapter tags consistently:

```text
docker-foundations
docker-images-compose
docker-data-stack
docker-security-ops
```

API notes use these chapter tags consistently:

```text
api-foundations
api-consuming
api-building
api-production-integration
```

Use the general tags `fivetran` and `learning` on Fivetran topic notes, plus exactly one main Fivetran chapter tag.

Use the general tags `docker` and `learning` on Docker topic notes, plus exactly one main Docker chapter tag.

Use the general tags `api` and `learning` on API topic notes, plus exactly one main API chapter tag.

Use the general tags `dbt` and `learning` on dbt topic notes, plus exactly one main dbt chapter tag unless the note is intentionally cross-cutting.

Example dbt topic frontmatter:

```yaml
---
status: seed
platform: dbt
area: Packages Macros and Advanced Reuse
topic_number:
tags:
  - dbt
  - dbt-packages-macros
  - learning
---
```

Do not remove existing frontmatter unless explicitly asked.

## Graph View

The graph should remain simple.

Preferred setup:

- Snowflake Learning Map links to chapter overview notes.
- dbt Learning Map links to dbt chapter overview notes.
- Fivetran Learning Map links to Fivetran chapter overview notes.
- Docker Learning Map links to Docker chapter overview notes.
- API Learning Map links to API chapter overview notes.
- Chapter overview notes link to topic notes.
- Comparisons and Decision Notes Overview links to comparison, decision, and scenario notes.
- Scenario/decision notes link to the few topic notes needed to reason through the situation.
- Topic notes link to their overview note and selected related topics.
- Use tags for Graph View color groups.

For cross-tool learning, prefer this pattern:

- Learning maps link to each other.
- Chapter overviews carry curated cross-tool links.
- Topic notes carry only the cross-tool links needed for practical reasoning.
- Decision, comparison, and scenario notes link to the mixed set of topics needed to make a recommendation.

Avoid file-based Graph View hacks or excessive cross-linking. Too many links make the graph noisy. A comparison or scenario note should usually link to 3-6 genuinely relevant topic notes, not every vaguely related note.

## Durable Notes Principle

The vault should contain distilled understanding, not raw conversation. Notes should preserve what will still be useful weeks later: mental models, decision rules, examples, trade-offs, pitfalls, and consultant talking points.

## Collaboration Pattern

When helping the learner with a topic, the agent should treat the conversation as a learning dialogue first and a note-writing task second.

For dbt, Fivetran, Docker, and API topics, keep the conversational teaching slightly shorter and more direct when possible. Lead with the mental model and essential practical trade-offs, use only the examples needed to make the concept clear, and expand when the learner asks follow-up questions. This preference applies to the learning conversation; durable vault notes should still follow the established topic-note structure.

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

- **Default to Mermaid diagrams.** Mermaid is the preferred visual medium because it is editable, lightweight, Git-friendly, and renders natively in Obsidian. When a topic would benefit from a visual, reach for Mermaid first.
- Mermaid fits most consultant topics well: flows, lifecycles, decision paths, dependency maps, sequence/threshold logic, and lightweight architecture sketches.
- Use embedded local images only as the exception — when a polished architecture diagram, UI screenshot, or vendor visual genuinely communicates the topic better than Mermaid could. Store local image files under `00 Home/assets/`.
- Default target is one useful visual per topic note when relevant and possible; assume Mermaid unless an image is clearly the stronger choice.
- Keep SQL/config snippets outside Mermaid unless a very short code-like label makes the diagram clearer; use normal fenced code blocks for copyable examples.
- A topic note may omit a visual only if no high-value visual is available; in that case, add a one-line note in `Visuals` stating this explicitly.
- Prioritize official sources (Snowflake, dbt, Fivetran, Docker, and Apache Airflow) for any external images.
- If official visuals are not available, use reputable sources.
- Keep source links in `Sources To Revisit` for each note using external visuals.

## Important Preference

The vault should feel familiar and consistent from note to note. Prioritize a stable structure over clever formatting.

# Vault Authoring Guide

This guide defines how to create and update durable content in this Obsidian vault. Read it before changing learning maps, chapter overviews, topic notes, comparisons, decision notes, scenarios, templates, tags, or links.

The vault is for consultant-level working knowledge of modern data engineering. It should help the learner recognize technologies, explain trade-offs, frame architecture decisions, and plan practical work without becoming a documentation dump.

## Authoring Principles

- Preserve the existing folder structure unless the user explicitly asks for a reorganization.
- Prefer stable, familiar note structures over clever formatting.
- Write for consultant working knowledge rather than exhaustive implementation depth.
- Distill durable understanding; do not save raw conversation.
- Keep technical areas distinct, but add curated cross-tool links when they clarify ownership, risk, cost, or implementation trade-offs.
- Do not edit the vault merely because a discussion occurred. Save or update notes when the user asks, or when the active task explicitly includes durable authoring.
- Preserve existing frontmatter unless the user explicitly asks to remove or replace it.

## Main Structure

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

Do not create new top-level learning or reasoning areas merely to mirror a curriculum. Add material only when it has durable value.

## Learning Maps and Chapters

The main navigation notes are:

```text
00 Home/Snowflake Learning Map.md
02 dbt/dbt Learning Map.md
03 Fivetran/Fivetran Learning Map.md
04 Docker/Docker Learning Map.md
05 APIs/API Learning Map.md
```

Each learning map links to chapter overview notes. Each chapter overview links to its topic notes. Each topic note links back to its chapter overview and to a small number of genuinely related topics.

### Snowflake chapters

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

### dbt chapters

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

Focus dbt notes on how a concept changes the analytics-engineering workflow, when to recommend it, and what governance, operational, cost, or banking-control implications matter.

Treat dbt packages as tools to evaluate, not packages to install by default.

- Must understand: `dbt_utils`, `codegen`, `audit_helper`, `dbt_project_evaluator`, and `dbt_expectations`.
- Strong situational value: `elementary`, `dbt_artifacts`, `dbt_date`, `dbt_external_tables`, Snowflake query-tag packages, `dbt_snowflake_monitoring`, and `dbt_constraints`.
- Finance or banking relevance: audit and reconciliation packages, observability packages, Data Vault packages such as `automate_dv` or `datavault4dbt` when the modeling strategy uses Data Vault, constraint packages, metadata-testing packages, and Snowflake cost or query-attribution packages.
- Use caution with source-specific, vendor-specific, lightly maintained, or business-logic-heavy packages. In regulated settings, consider maintenance, license, security review, version pinning, Fusion compatibility, Snowflake compatibility, and support ownership.

Advanced dbt topics worth explicit coverage include Jinja fundamentals, macros, custom generic tests, adapter dispatch, hooks, operations, package governance, project dependencies, model access, model contracts, model versions, dbt Mesh, semantic models, dbt State, and CI deferral.

### Fivetran chapters

```text
03 Fivetran/01 Foundations and Platform Mental Model/
03 Fivetran/02 Connectors and Sync Behavior/
03 Fivetran/03 Destination Data History and Schema Change/
03 Fivetran/04 Fivetran with Snowflake and dbt/
03 Fivetran/05 Security Governance and Production Operations/
03 Fivetran/06 Cost and Consultant Decision-Making/
```

Focus Fivetran notes on source and connector fit, sync and destination behavior, schema and data-quality boundaries, security and operations, usage cost, and the ownership boundary with Snowflake and dbt. Do not force code into these notes. Use short configuration, destination-row, API, Terraform, SQL, calculation, or checklist examples only when they improve recognition.

### Docker chapters

```text
04 Docker/01 Foundations and Everyday Docker/
04 Docker/02 Reproducible Images and Docker Compose/
04 Docker/03 Data Stack Integration Patterns/
04 Docker/04 Security Operations and Team Standards/
```

Keep the compact Docker curriculum focused on working fluency in a Snowflake, dbt Core, Airflow, and Python environment: reproducible images, everyday container operation, Docker Compose, dependency and secret boundaries, debugging, security, CI promotion, and production-fit judgment.

- Preserve the dedicated Data Stack Integration Patterns chapter.
- Prefer merging related mechanics into the existing topics over adding narrow new topics.
- Treat Snowflake as an external managed service in the normal case.
- Do not expand into Kubernetes or deep container internals unless the learner asks.

### API chapters

```text
05 APIs/01 Foundations and API Literacy/
05 APIs/02 Consuming APIs for Data Pipelines/
05 APIs/03 Designing and Building APIs/
05 APIs/04 Production Security and Data Stack Integration/
```

Teach conversation fluency first, then consuming APIs, building with FastAPI and Pydantic, and production judgment. Emphasize data-engineering concerns: pagination, incremental state, rate limits, retries, idempotency, schema drift, reconciliation, service identity, Snowflake data access, observability, Docker deployment, and API-versus-file, CDC, or connector decisions. Do not turn the curriculum into front-end development, deep protocol internals, or a framework reference.

## Topic Notes

Use the existing templates:

```text
90 Templates/Topic Note Template.md
90 Templates/Fivetran Topic Note Template.md
90 Templates/Docker Topic Note Template.md
```

Keep the section order defined by the applicable template. A developed topic note should generally include:

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

It may also include Related Decision Notes.

### Consistency rules

- Write `How It Works` as a simple numbered flow, normally four to eight steps.
- Include practical pitfalls connected to cost, performance, governance, security, reliability, or operations.
- Include a meaningful `When to Recommend What (Decision Table)` when a note moves beyond a seed placeholder.
- After developing a topic note, add one to three curated links to relevant comparison or decision notes when they exist.
- Avoid direct links from topic notes to scenarios unless the scenario is exceptionally central. Let scenarios link to topics and rely on backlinks.
- Create or update a reasoning note only when the discussion produced durable recommendation material. Do not create reasoning notes merely to mirror topic coverage.
- Keep notes practical and scannable. Prefer concise explanations, small comparison tables, short examples, and clear decision rules.
- Use short, readable SQL or configuration snippets. Avoid large demo projects, code monoliths, exhaustive syntax references, or deep internals by default.

## Comparisons, Decision Notes, and Scenarios

Keep one shared reasoning layer under `80 Comparisons and Decision Notes/` for Snowflake, dbt, Fivetran, Docker, APIs, and cross-tool questions.

Organize it by note type and then domain:

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

API comparisons and decision notes belong under the existing `Cross-Tool/` folders.

### Naming and tags

- Comparison file and H1: `Comparison - <topic>`; tag: `note-comparison`.
- Decision-note file and H1: `Decisions - <topic>`; tag: `note-decision`.
- Client-scenario file and H1: `Scenario - <client problem>`; tag: `note-scenario`.

Keep `80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview.md` current. Group its links under Comparisons, Decision Notes, and Client Scenarios, with Snowflake, dbt, Fivetran, Docker, and Cross-Tool subheadings as appropriate.

### Epistemic safeguards

Reasoning notes are aids, not authoritative answer sources.

- Comparisons are synthesized option-discovery material whose technical claims may need current verification.
- Decision notes are diagnostic frameworks, not predetermined answers.
- Client scenarios are synthetic exercises, not evidence that their recommendations apply to a new situation.
- Write assumptions explicitly.
- Include alternative defensible recommendations and the conditions that favor them.
- State what would change the recommendation.
- Identify claims that require external verification.
- Include common ways the note could be misapplied.
- Do not present one polished scenario outcome as universally correct.

When authoring or revising these notes, prefer metadata that makes their role visible, such as `note_type`, `epistemic_status`, `use_as`, `validation_status`, and `last_verified`. Add `last_verified` only after actual verification.

### Work-oriented response templates

`Architecture Decision Template`, `Technical Spike Template`, and `Delivery Plan Template` structure chat responses and working drafts by default. Their use does not authorize creating a durable vault note or imply a storage location.

- Save a work-oriented artifact only when the user explicitly asks and either names a target path or agrees on a durable generalized destination.
- If an architecture outcome becomes a reusable cross-client decision framework, distill it into a `Decisions - <topic>` note under the appropriate `80 Comparisons and Decision Notes/Decision Notes/<domain>/` folder rather than storing a scenario-specific record as precedent.
- Save spikes and delivery plans only when their generalized method or evidence will remain useful; otherwise leave them in the conversation.
- Never place sanitized-but-situational work drafts in the reasoning layer merely because they used one of these templates.

## Obsidian Links

Use normal Obsidian wikilinks with forward slashes:

```md
[[01 Snowflake/04 Data Engineering/21 Dynamic Tables]]
```

Never use Windows backslashes in wikilinks. They can create duplicate blank notes in Graph View.

### Snowflake and dbt

- dbt topic notes primarily link to their dbt chapter overview and closely related dbt topics.
- Add Snowflake links only when Snowflake materially changes the recommendation, implementation, or risk discussion.
- Useful Snowflake connections often include warehouses and cost, RBAC, schemas and databases, Dynamic Tables, Streams and Tasks, Snowflake Tasks, query history, Account Usage, masking and row access policies, object tags, data loading, or dbt Projects on Snowflake.
- Prefer curated Snowflake links in dbt chapter overviews over repeated links in every topic.
- Keep `01 Snowflake/04 Data Engineering/25 dbt on Snowflake.md` as the main Snowflake-side bridge into dbt.
- Do not add cross-links merely because both tools belong to the modern data stack.

### Fivetran

- Fivetran topic notes primarily link to their chapter overview and closely related Fivetran topics.
- Add Snowflake links when destination identity, RBAC, networking, schemas, warehouses, table types, loading cost, monitoring, or recovery changes the recommendation.
- Add dbt links when source freshness, staging, tests, reconciliation, orchestration, or downstream contracts change the recommendation.
- Prefer curated cross-tool links from the Fivetran with Snowflake and dbt chapter overview.
- Keep `02 dbt/08 dbt on Snowflake and Finance Patterns/81 Fivetran to Snowflake to dbt Flow.md` as the dbt-side bridge into Fivetran.

### Docker

- Docker topic notes primarily link to their chapter overview and closely related Docker topics.
- Add Snowflake links when authentication, networking, certificates, proxies, service roles, query attribution, or external-service boundaries matter.
- Add dbt links when container execution affects dependencies, profiles, artifacts, CI, orchestration, or production jobs.
- Add Airflow context when Compose services, custom images, providers, task isolation, logs, or executor boundaries matter.
- Prefer curated cross-tool links from the Data Stack Integration Patterns overview.
- Do not imply that Docker Compose alone makes an Airflow deployment production-ready.

### APIs

- API topic notes primarily link to their chapter overview and closely related API topics.
- Add Snowflake links when service identity, SQL execution, REST resource management, external access, Snowpark Container Services, workload isolation, query attribution, latency, or cost changes the recommendation.
- Add Fivetran links when managed API connectors, source coverage, sync behavior, schema handling, or build-versus-buy ownership matters.
- Add Docker links when packaging, secrets, networking, health, CI promotion, or runtime operations matter.
- Add dbt links when curated serving models, contracts, tests, freshness, or precomputation materially shape the API.
- Do not imply that exposing a Snowflake query through HTTP makes it an operationally suitable API.

Across all tools, link only when the relationship improves practical reasoning about ownership, reproducibility, security, governance, cost, reliability, performance, or operations.

## Tags and Frontmatter

Snowflake chapter tags:

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

dbt chapter tags:

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

Fivetran chapter tags:

```text
fivetran-foundations
fivetran-connectors-sync
fivetran-data-schema
fivetran-snowflake-dbt
fivetran-security-operations
fivetran-cost-decisions
```

Docker chapter tags:

```text
docker-foundations
docker-images-compose
docker-data-stack
docker-security-ops
```

API chapter tags:

```text
api-foundations
api-consuming
api-building
api-production-integration
```

For Snowflake, dbt, Fivetran, Docker, and API topic notes, use the platform tag, `learning`, and exactly one main chapter tag unless the note is intentionally cross-cutting.

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

Do not claim that a note is current merely because a date field exists. Set `last_verified` only after reviewing the material against appropriate sources.

Use metadata consistently:

- `status: seed` means the note is a placeholder, incomplete, or not ready to support an answer.
- `status: draft` means a working output is still being shaped and is not durable guidance or evidence.
- `status: reference` means a condensed or archived reference that may supply background but is a lead rather than current evidence unless independently validated.
- `status: active` means the note is developed enough for use, subject to its validation and freshness metadata.
- `status: hub` means the note is primarily navigation and synthesis.
- `status: deprecated` means the note should not guide new work; link to its replacement when one exists.
- `validation_status: unvalidated` means material claims have not been systematically checked.
- `validation_status: source-supported` means material factual claims are backed by cited appropriate sources and assumptions are explicit.
- `validation_status: reviewed` means the whole note has also been checked for internal consistency, applicability, links, and metadata.
- `validation_status: superseded` means newer evidence or guidance has replaced the note's material conclusions.

Promote `seed` to `active` only when the substantive sections are developed and obvious placeholders are resolved. Promote validation independently: an active note may still be unvalidated, and therefore remains a lead rather than evidence. Set or refresh `last_verified` only when the material claims were actually checked during that review.

## Graph View

Keep the graph simple:

- Learning maps link to chapter overviews.
- Chapter overviews link to topic notes.
- The Comparisons and Decision Notes overview links to comparison, decision, and scenario notes.
- Scenario and decision notes link to the few topic notes needed for their reasoning.
- Topic notes link to their overview and selected related topics.
- Learning maps can link to each other for cross-tool navigation.
- Chapter overviews carry curated cross-tool links.
- Tags provide Graph View color groups.

Avoid file-based Graph View hacks and excessive cross-linking. A comparison or scenario usually needs only three to six genuinely relevant topic links.

## Durable Authoring Workflow

Treat learning conversations as dialogue first and note-writing second.

1. Teach or investigate the concept conversationally.
2. Answer follow-up questions and help the learner test the reasoning.
3. Track useful distinctions, examples, decision rules, and unresolved questions.
4. At a natural stopping point, summarize the durable learning.
5. Remove tangents, repetition, mistakes, and weak explanations.
6. When saving is requested, update the relevant note with only reusable material.
7. Add curated links and short readable snippets where helpful.
8. Add or update a comparison, decision, or scenario note only if the discussion produced durable reasoning material.
9. Commit and push only when the user asks or the active repository workflow explicitly requires it.

The finished note should read as a clean consultant reference, not a transcript. For dbt, Fivetran, Docker, and API topics, keep conversational teaching concise and lead with the mental model and essential trade-offs. The durable note should still follow the stable template.

## Visuals

Use visuals when they materially improve learning speed.

- Default to Mermaid because it is editable, lightweight, Git-friendly, and renders in Obsidian.
- Mermaid is well suited to flows, lifecycles, decision paths, dependency maps, sequence or threshold logic, and lightweight architecture diagrams.
- Use embedded local images only when a polished architecture diagram, UI screenshot, or vendor visual communicates the subject better than Mermaid.
- Store local images under `00 Home/assets/`.
- Aim for one useful visual per developed topic when relevant and possible.
- Keep SQL and configuration snippets outside Mermaid unless a very short code-like label improves the diagram.
- If no high-value visual exists, add a one-line statement in `Visuals` explaining that omission.
- Prefer official Snowflake, dbt, Fivetran, Docker, or Apache Airflow sources for external images, followed by reputable sources when official material is unavailable.
- Record external visual sources in `Sources To Revisit`.

## Git Workflow

Before editing, inspect the worktree:

```bash
git status --short --branch
```

Preserve unrelated or existing user changes. Do not commit Obsidian local workspace state; `workspace.json` is intentionally ignored.

After meaningful, authorized changes:

```bash
git add <specific-files>
git commit -m "Describe the learning note update"
git push
```

Stage specific files rather than `git add .` when unrelated changes exist.
Do not commit or push when the user asks only for review, explanation, or a local draft.

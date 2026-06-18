# Visual Asset Guidelines

Mermaid diagrams are the default visual medium for topic notes — they are editable, lightweight, Git-friendly, and render natively in Obsidian. Reach for Mermaid first.

Use this folder only for the exception case: local **image** assets embedded when a polished architecture diagram, UI screenshot, or vendor visual communicates the topic better than Mermaid could.

## When to Use an Image Instead of Mermaid

- A polished official architecture diagram or vendor visual already exists and is clearer than a hand-built Mermaid sketch.
- A UI screenshot is needed to show an actual product surface.
- The concept is genuinely hard to express as nodes/edges (e.g. a detailed reference architecture).

For flows, lifecycles, decision paths, dependency maps, and threshold/sequence logic, prefer Mermaid in the note itself — no file needed here.

## Naming Convention

`<platform>-<area>-<topic-number>-<short-slug>.<ext>`

Examples:
- `snowflake-core-01-virtual-warehouse-architecture.png`
- `snowflake-core-02-micro-partition-pruning.png`

## Source Rules

1. Prioritize official sources (Snowflake, dbt, Fivetran).
2. If official visuals are unavailable, use reputable secondary sources.
3. Keep a source link in the topic note under `Sources To Revisit`.
4. Keep visuals focused on architecture, flow, or decision mechanics.

## Topic Note Usage Rule

- Aim for one useful visual per topic note when relevant — a Mermaid diagram by default, a local image only when it is clearly the stronger choice.
- If no useful visual is available yet, keep the `Visuals` section and state that explicitly.
- Always include a source attribution URL in the note when using an external image.

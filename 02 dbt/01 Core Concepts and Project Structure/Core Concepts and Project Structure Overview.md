---
status: hub
platform: dbt
area: Core Concepts and Project Structure
tags:
  - dbt
  - dbt-core-projects
  - map
---

# Core Concepts and Project Structure Overview

> Build the dbt mental model: what it does, how a project is organized, and how models, sources, environments, commands, and artifacts fit together.

> [!abstract] Chapter outcome
> You should be able to explain the dbt workflow end to end, recognize its main project components, and identify where platform, governance, and operating-model decisions sit.

## Topics

- [[02 dbt/01 Core Concepts and Project Structure/01 What dbt Is and Is Not|01 - What dbt Is and Is Not]]
- [[02 dbt/01 Core Concepts and Project Structure/02 dbt Core Fusion dbt Platform and dbt Projects on Snowflake|02 - dbt Core, Fusion, dbt Platform, and dbt Projects on Snowflake]]
- [[02 dbt/01 Core Concepts and Project Structure/03 Project Anatomy and dbt_project.yml|03 - Project Anatomy and dbt_project.yml]]
- [[02 dbt/01 Core Concepts and Project Structure/04 Environments Profiles Targets and Credentials|04 - Environments, Profiles, Targets, and Credentials]]
- [[02 dbt/01 Core Concepts and Project Structure/05 Models ref source and the DAG|05 - Models, ref(), source(), and the DAG]]
- [[02 dbt/01 Core Concepts and Project Structure/06 Commands and Artifacts|06 - Commands and Artifacts]]
- [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness|07 - Sources and Source Freshness]]
- [[02 dbt/01 Core Concepts and Project Structure/08 Seeds and Static Reference Data|08 - Seeds and Static Reference Data]]
- [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking|09 - Snapshots and Historical Change Tracking]]
- [[02 dbt/01 Core Concepts and Project Structure/10 Documentation Lineage and Exposures|10 - Documentation, Lineage, and Exposures]]

## Chapter Map

```mermaid
flowchart LR
    A["1–2<br/>Position dbt"] --> B["3–5<br/>Structure the project"]
    B --> C["6<br/>Build and inspect"]
    C --> D["7–9<br/>Manage inputs and history"]
    D --> E["10<br/>Explain lineage and ownership"]

    classDef foundation fill:#E8F0FE,stroke:#4C6EF5,color:#172B4D
    classDef build fill:#E6FCF5,stroke:#0CA678,color:#163A2D
    classDef operate fill:#FFF4E6,stroke:#F08C00,color:#4A2A00
    classDef communicate fill:#F3F0FF,stroke:#7950F2,color:#2B1B54

    class A foundation
    class B,C build
    class D operate
    class E communicate
```

## Topic Summaries

| Topic | What it unlocks |
|---|---|
| [[02 dbt/01 Core Concepts and Project Structure/01 What dbt Is and Is Not|01 - What dbt Is and Is Not]] | Separates transformation workflow from ingestion, storage, BI, orchestration, and governance. |
| [[02 dbt/01 Core Concepts and Project Structure/02 dbt Core Fusion dbt Platform and dbt Projects on Snowflake|02 - Core, Fusion, Platform, and Snowflake]] | Compares execution engines and control planes without losing sight of where SQL runs. |
| [[02 dbt/01 Core Concepts and Project Structure/03 Project Anatomy and dbt_project.yml|03 - Project Anatomy and dbt_project.yml]] | Explains the folders, configuration, and conventions that make a project maintainable. |
| [[02 dbt/01 Core Concepts and Project Structure/04 Environments Profiles Targets and Credentials|04 - Environments, Profiles, Targets, and Credentials]] | Connects environment separation to warehouses, secrets, and role design. |
| [[02 dbt/01 Core Concepts and Project Structure/05 Models ref source and the DAG|05 - Models, ref(), source(), and the DAG]] | Establishes the core mental model for dependencies, build order, and lineage. |
| [[02 dbt/01 Core Concepts and Project Structure/06 Commands and Artifacts|06 - Commands and Artifacts]] | Shows how dbt builds, validates, and records what happened. |
| [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness|07 - Sources and Source Freshness]] | Connects upstream data availability to downstream reliability. |
| [[02 dbt/01 Core Concepts and Project Structure/08 Seeds and Static Reference Data|08 - Seeds and Static Reference Data]] | Defines the narrow, controlled use case for versioned static data. |
| [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking|09 - Snapshots and Historical Change Tracking]] | Introduces history capture for mutable source records. |
| [[02 dbt/01 Core Concepts and Project Structure/10 Documentation Lineage and Exposures|10 - Documentation, Lineage, and Exposures]] | Turns project metadata into shared context about lineage, use, and ownership. |

## How To Use This Area

Follow the topics in order for a first pass. Later, use the table as a quick reference and jump directly to the concept you need.

The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and each topic links back to this hub so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/02 Modeling Patterns and Layering/Modeling Patterns and Layering Overview|Next: Modeling Patterns and Layering]]

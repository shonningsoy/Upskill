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

> Foundational dbt concepts: what dbt is, how projects are structured, and how models, sources, commands, targets, and artifacts fit together.

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

## Topic Summaries

### [[02 dbt/01 Core Concepts and Project Structure/01 What dbt Is and Is Not|01 - What dbt Is and Is Not]]

Separates transformation workflow from ingestion, storage, BI, orchestration, and governance tooling.

### [[02 dbt/01 Core Concepts and Project Structure/02 dbt Core Fusion dbt Platform and dbt Projects on Snowflake|02 - dbt Core, Fusion, dbt Platform, and dbt Projects on Snowflake]]

Helps compare execution engines and control planes without confusing where SQL actually runs.

### [[02 dbt/01 Core Concepts and Project Structure/03 Project Anatomy and dbt_project.yml|03 - Project Anatomy and dbt_project.yml]]

Explains folders, naming, configs, model paths, and how a dbt project becomes maintainable.

### [[02 dbt/01 Core Concepts and Project Structure/04 Environments Profiles Targets and Credentials|04 - Environments, Profiles, Targets, and Credentials]]

Critical for dev/test/prod separation, warehouse choice, secrets, and role design.

### [[02 dbt/01 Core Concepts and Project Structure/05 Models ref source and the DAG|05 - Models, ref(), source(), and the DAG]]

Core mental model for dependency management and lineage.

### [[02 dbt/01 Core Concepts and Project Structure/06 Commands and Artifacts|06 - Commands and Artifacts]]

Covers run, test, build, compile, deps, seed, snapshot, manifest.json, run_results.json, and catalog.json.

### [[02 dbt/01 Core Concepts and Project Structure/07 Sources and Source Freshness|07 - Sources and Source Freshness]]

Connects raw data availability to downstream reliability.

### [[02 dbt/01 Core Concepts and Project Structure/08 Seeds and Static Reference Data|08 - Seeds and Static Reference Data]]

Useful for small controlled mappings, but risky for sensitive or changing production data.

### [[02 dbt/01 Core Concepts and Project Structure/09 Snapshots and Historical Change Tracking|09 - Snapshots and Historical Change Tracking]]

Important for mutable source data and slowly changing dimensions.

### [[02 dbt/01 Core Concepts and Project Structure/10 Documentation Lineage and Exposures|10 - Documentation, Lineage, and Exposures]]

Turns dbt from SQL execution into a knowledge and ownership layer.

## How To Use This Area

Use this note as the local hub for this dbt chapter. The global [[02 dbt/dbt Learning Map|dbt Learning Map]] links here, and the topic notes link back here so Graph View stays readable.

## Related Areas

- [[02 dbt/dbt Learning Map|dbt Learning Map]]
- [[02 dbt/02 Modeling Patterns and Layering/Modeling Patterns and Layering Overview|Next: Modeling Patterns and Layering]]

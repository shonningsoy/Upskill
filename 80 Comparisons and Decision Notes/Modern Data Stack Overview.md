# Modern Data Stack Overview

## Short Version

- **Fivetran** usually handles extraction and loading from source systems.
- **Snowflake** usually handles storage, compute, governance, and analytical querying.
- **dbt** usually handles SQL-based transformations, testing, documentation, and analytics engineering workflow.
- **Airflow** usually coordinates when and in what order data jobs run.
- **Python** usually handles custom processing, automation, APIs, and utilities that do not fit cleanly in SQL.
- **Docker** packages and runs the software environment used by dbt Core, Airflow, and Python; Snowflake normally remains an external managed service.
- **APIs** define software contracts for interactive access and automation; they may be consumed as data sources or produced as governed data products.

## Consultant Frame

A useful client discussion usually separates these questions:

- Where does the data come from?
- Where should the data live?
- Where should transformations happen?
- Who owns quality, documentation, and governance?
- How will cost, access, and operations be controlled?
- Which runtime dependencies should be standardized in container images, and which configuration or credentials must stay external?
- Does the interaction need an API, file, CDC stream, event, data share, or direct query pattern?

## Runtime View

```mermaid
flowchart LR
    S["Sources"] --> I["Ingestion"]
    I --> W[("Snowflake")]
    W --> D["dbt Core transformations"]
    A["Airflow"] --> I
    A --> D
    A --> P["Python jobs"]
    P --> W
    E["External APIs"] --> P
    W --> Q["Governed data product API"]
    X["Docker images and containers"] -. standardize runtime .-> A
    X -. standardize runtime .-> D
    X -. standardize runtime .-> P
```

## Related Reasoning Notes

- [[80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview]]
- [[04 Docker/Docker Learning Map|Docker Learning Map]]
- [[05 APIs/API Learning Map|API Learning Map]]

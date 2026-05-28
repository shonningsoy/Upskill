# Modern Data Stack Overview

## Short Version

- **Fivetran** usually handles extraction and loading from source systems.
- **Snowflake** usually handles storage, compute, governance, and analytical querying.
- **dbt** usually handles SQL-based transformations, testing, documentation, and analytics engineering workflow.

## Consultant Frame

A useful client discussion usually separates these questions:

- Where does the data come from?
- Where should the data live?
- Where should transformations happen?
- Who owns quality, documentation, and governance?
- How will cost, access, and operations be controlled?

## Related Reasoning Notes

- [[80 Comparisons and Decision Notes/Comparisons and Decision Notes Overview]]

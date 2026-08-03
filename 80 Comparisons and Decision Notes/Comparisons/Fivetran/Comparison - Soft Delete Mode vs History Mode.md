---
tags:
  - note-comparison
---

# Comparison - Soft Delete Mode vs History Mode

## Short Answer

Use **soft delete mode** for a simple current-state landing table with one row per source key.

Use **history mode** only when observed row versions have durable analytical or audit value that justifies extra rows, MAR, and query complexity.

## Comparison Table

| Dimension | Soft delete mode | History mode |
|---|---|---|
| Primary purpose | Represent latest observed source state | Preserve observed versions over time |
| Row grain | One destination row per source key | Multiple versions per source key |
| Updates | Existing destination row is updated | Old version is closed and a new active version is inserted |
| Deletes | `_fivetran_deleted` identifies detected deletes | Version metadata identifies current and prior state |
| Query pattern | Exclude deleted rows where appropriate | Filter `_fivetran_active` or use an as-of validity predicate |
| Cost considerations | Lower row growth for frequently updated records | Repeated changes create versions and can increase MAR and storage |
| Consultant recommendation | Default raw current-state pattern | Selective use for justified history requirements |

## Decision Rules

- Confirm the connector and table support the desired mode before designing downstream models.
- Use history mode only where the business needs source-record history, not merely because history sounds safer.
- Treat a mode change as a data-contract migration because system columns and table grain change.
- Do not claim history mode reconstructs changes Fivetran never observed.

## Related Learning Topics

- [[03 Fivetran/03 Destination Data History and Schema Change/12 Soft Delete Mode vs History Mode]]
- [[03 Fivetran/03 Destination Data History and Schema Change/13 Keys Deletes and Fivetran System Columns]]
- [[03 Fivetran/03 Destination Data History and Schema Change/16 Data Contracts Completeness and Reconciliation]]
- [[03 Fivetran/06 Cost and Consultant Decision-Making/29 Monthly Active Rows]]

## Related Scenarios

- No dedicated scenario note yet.

## Sources To Revisit

- [Fivetran Sync Modes](https://fivetran.com/docs/core-concepts/sync-modes)
- [Fivetran History Mode](https://fivetran.com/docs/core-concepts/sync-modes/history-mode)
- [Fivetran System Columns and Tables](https://fivetran.com/docs/core-concepts/system-columns-and-tables)

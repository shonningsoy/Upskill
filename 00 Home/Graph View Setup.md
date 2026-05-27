# Graph View Setup

Use tag-based groups for Snowflake chapters. This keeps the setup simple and avoids brittle filename or folder-path matching.

## Snowflake Chapter Groups

Add these in Graph View settings under **Groups**.

| Chapter | Query |
|---|---|
| Core Architecture and Concepts | `tag:#sf-core-architecture` |
| Performance and Optimization | `tag:#sf-performance` |
| Security and Governance | `tag:#sf-security-governance` |
| Data Engineering | `tag:#sf-data-engineering` |
| Advanced Analytics and AI | `tag:#sf-analytics-ai` |
| Cost Management and Operations | `tag:#sf-cost-ops` |
| Ecosystem and Integration | `tag:#sf-ecosystem-integration` |

## Notes

The tags live in the YAML frontmatter of each Snowflake topic and overview note. If a node looks grey, first check whether the note is an unresolved link or duplicate note. Real topic notes should open normally and contain frontmatter at the top.

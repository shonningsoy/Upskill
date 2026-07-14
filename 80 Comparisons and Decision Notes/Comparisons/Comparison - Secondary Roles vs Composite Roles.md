---
tags:
  - note-comparison
---

# Comparison - Secondary Roles vs Composite Roles

## Short Answer

Use **composite roles** when audit clarity and regulatory compliance matter (banks, healthcare, enterprise).
Use **secondary roles** when flexibility and speed matter more than strict audit attribution (internal analytics teams, startups).

## Comparison Table

| Dimension | Secondary Roles | Composite Roles |
|---|---|---|
| Primary purpose | Activate all granted roles simultaneously for convenience | Purpose-built role inheriting from multiple roles for explicit cross-domain access |
| How it works | `USE SECONDARY ROLES ALL` activates union of all granted role privileges | A single role is created that inherits from the needed child roles |
| Audit trail | Fuzzy — `QUERY_HISTORY` shows `secondary_role_name = 'ALL'`, not which role provided access | Clean — one named role in audit trail with documented justification |
| Setup effort | Zero — user just runs a command | Requires creating the role, granting child roles, assigning to users |
| Governance | Hard to explain to regulators ("which role granted access?") | Easy — "this role exists because regulatory reporting requires Finance + Risk" |
| Flexibility | Maximum — user gets everything they're granted automatically | Scoped — user gets exactly what the composite role provides |
| Risk of over-access | Higher — all granted roles active regardless of current task | Lower — composite role is purpose-scoped |
| User experience | Easy — no switching, no planning | Easy — one role covers the job function |
| Revocation | Revoke any child role and it's gone from the union | Revoke the composite role or remove a child role from it |
| Forensic investigation | Must cross-reference grants to determine which secondary role provided the privilege | Direct — the composite role's definition shows exactly what access it provides |

## Decision Rules

- If regulators will ask "show me which role granted access to this table" → composite roles.
- If the cross-domain need is permanent and well-understood → composite role (named, documented, approved).
- If the need is temporary or exploratory → secondary roles with monitoring may be acceptable.
- If the organization uses SCIM provisioning → composite roles map cleanly to Entra ID groups; secondary roles don't.
- If audit simplicity is a priority → composite roles (one role name in every log entry).
- If the team is small and trust is high → secondary roles are fine; don't over-engineer.

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/Security and Governance Overview]]
- [[01 Snowflake/06 Cost Management and Operations/45 Account Usage Views]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Scenario - New Regulatory Team Needs Cross-Domain Data]]

---
tags:
  - note-comparison
---

# Comparison - Network Policy vs Private Connectivity

## Short Answer

Use a **Network Policy** for free, baseline IP filtering on any edition — restrict which source IPs may connect.
Use **Private Connectivity** when a regulated client needs traffic to never touch the public internet — at the cost of Business Critical edition and joint cloud-team setup.

They are complementary layers, not alternatives. The strongest posture uses both: private connectivity for the path, and a network policy blocking public IPs.

## Comparison Table

| Dimension | Network Policy | Private Connectivity |
|---|---|---|
| Primary purpose | Filter inbound connections by source IP | Route traffic over the cloud provider's private backbone |
| Public internet | Traffic still rides the internet, just filtered | Traffic bypasses the internet entirely |
| Edition / cost | Free on all editions | Typically requires Business Critical (higher cost) |
| Setup effort | Simple SQL (rules + policy) | Joint effort with cloud team (VPC endpoint, DNS, private URL) |
| Strengths | Free, fast, granular per-user override | True network isolation; satisfies strict regulators |
| Limits | Brittle to IP changes; not truly private | Cost; doesn't cover replication/outbound by default |
| Governance considerations | Good baseline; not authentication | Maps to "no public exposure" audit controls |
| Consultant recommendation | Always apply as a baseline | Add when compliance mandates zero internet exposure |

## Decision Rules

- Always apply a network policy — it's free and there's no reason to skip it (just avoid self-lockout).
- Add private connectivity when a legal/compliance framework genuinely requires no public-internet path.
- Use a user-level network policy to give service accounts a narrower IP range than humans (remember: it replaces, not stacks).
- With private connectivity, also block the public URL via a network policy, or the public door stays open.
- Neither is authentication — always layer RBAC + MFA/SSO on top.

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/16 Network Policies and Private Connectivity]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]
- [[01 Snowflake/03 Security and Governance/17 Tri-Secret Secure and Customer-Managed Keys]]

## Related Scenarios

- 

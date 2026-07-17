---
tags:
  - note-comparison
---

# Comparison - Default Encryption vs Tri-Secret Secure

## Short Answer

Use **Default Encryption** for virtually every client — data is already AES-256 encrypted at rest, free, automatic, zero operational burden.
Use **Tri-Secret Secure** only when the requirement is *key ownership and revocability* (HYOK), not encryption strength — and the client can operate a KMS maturely.

The crucial framing: both use identical AES-256. The difference is **who controls the key**, not how strong the encryption is.

## Comparison Table

| Dimension | Default Encryption | Tri-Secret Secure (CMK) |
|---|---|---|
| Encryption strength | AES-256 | AES-256 (identical) |
| Who holds the key | Snowflake only | Snowflake + customer (composite) |
| Can revoke vendor access? | No | Yes — the kill switch |
| Edition required | All editions (free, automatic) | Business Critical+ |
| Operational burden | None (fully managed) | Customer runs KMS, key lifecycle, HA, break-glass |
| Self-destruct risk | None | Real — lost/misconfigured key = data unrecoverable |
| Availability dependency | Snowflake only | Also depends on customer KMS uptime |
| Who it's for | Everyone (baseline) | Regulated FS / healthcare / gov needing HYOK |

## Decision Rules

- Default is enough for most clients — don't upsell Tri-Secret on "security strength," which is identical.
- Recommend Tri-Secret only when the driver is control/revocability: a mandate to hold your own key, provable vendor access cuts, or cryptographic-shredding obligations.
- Treat it as a Business Critical package deal — bundle with private connectivity (topic 16) and the broader compliance posture.
- Gate on operational maturity: without HA KMS, disciplined key custody, and a tested break-glass runbook, Tri-Secret increases risk.

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/17 Tri-Secret Secure and Customer-Managed Keys]]
- [[01 Snowflake/03 Security and Governance/16 Network Policies and Private Connectivity]]
- [[01 Snowflake/03 Security and Governance/12 RBAC Roles and Privileges]]

## Related Scenarios

- [[80 Comparisons and Decision Notes/Client Scenarios/Snowflake/Scenario - Client Needs to Revoke Vendor Key Access]]

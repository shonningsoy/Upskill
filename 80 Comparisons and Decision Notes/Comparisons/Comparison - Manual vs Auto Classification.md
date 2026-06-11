---
tags:
  - note-comparison
---

# Comparison - Manual vs Auto Classification

## Short Answer

Use **Manual classification** when the estate is small or stable, you want human review before tags are applied, or you only need a point-in-time discovery audit.
Use **Auto-classification (classification profiles)** when the estate is large or fast-changing and you need sensitive-data coverage to maintain itself as new tables and columns land.

## Comparison Table

| Dimension | Manual classification | Auto-classification (profile) |
|---|---|---|
| Primary purpose | One-time / on-demand discovery with human review | Continuous, self-maintaining classification at scale |
| How tags are applied | You review recommendations, then apply | Applied automatically on a schedule (`auto_tag`) |
| Strengths | Full control, accuracy review, no recurring cost | Scales, catches new/changed data, low ongoing effort |
| Limits | Snapshot in time; doesn't track drift; manual effort | Probabilistic tags applied without per-run review |
| Cost considerations | Pay only when you run it | Recurring credit cost; grows with estate size/churn |
| Governance considerations | Human sign-off before tags exist | Must audit results; risk of silent false positives/negatives |
| Consultant recommendation | Small/stable schemas, audits, high-stakes review | Large/churning estates needing durable PII coverage |

## Decision Rules

- Known, stable sensitive columns on a small schema → consider skipping classification and tagging manually.
- Need defensible compliance evidence at a point in time → manual classification with review.
- New tables arrive regularly and must be protected without manual intervention → auto-classification profile scoped to sensitive schemas.
- Either approach is only useful if paired with tag-based masking or access policies — labeling alone protects nothing.
- Scope auto-classification to schemas that actually hold sensitive data to control recurring cost.

## Related Learning Topics

- [[01 Snowflake/03 Security and Governance/15 Data Classification]]
- [[01 Snowflake/03 Security and Governance/14 Column-level Masking Policies]]
- [[01 Snowflake/03 Security and Governance/18 Object Tagging]]

## Related Scenarios

- 

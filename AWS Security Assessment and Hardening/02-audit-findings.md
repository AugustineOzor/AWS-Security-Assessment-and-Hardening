# 02 · Audit Findings

**Navigate:** [README](../README.md) · [Assets](01-asset-inventory.md) · [Findings](02-audit-findings.md) · [Risks](03-risk-register.md) · [Controls](04-control-catalog.md)

Gaps found by the audit and follow-up assessments, tracked from discovery to closure. Each finding points to the assets affected, the risk it creates and the controls that address it.

## Summary

| ID | Title | Severity | Status | Source | Owner | Due |
|---|---|---|---|---|---|---|
| [FND-001](#fnd-001) | Publicly accessible S3 buckets holding client data | Critical | In Remediation | External Audit | Cloud Platform Manager | 2026-10-24 |
| [FND-002](#fnd-002) | Overly permissive IAM policies with wildcard permissions | Critical | Open | External Audit | Cloud Security Lead | 2026-11-07 |
| [FND-003](#fnd-003) | Multi-factor authentication not enforced for all console users | High | In Progress | Internal Audit | Cloud Security Lead | 2026-11-14 |
| [FND-004](#fnd-004) | Security groups allow SSH and RDP from any address | High | Open | Continuous Monitoring | Network Security Lead | 2026-11-21 |
| [FND-005](#fnd-005) | Unencrypted EBS volumes on internal tooling instances | Medium | Closed | Self-Assessment | Infrastructure Manager | 2026-09-30 |

## Details

### <a id="fnd-001"></a>**FND-001** — Publicly accessible S3 buckets holding client data

**Description:** The audit found 9 S3 buckets across client accounts readable by anyone on the internet, including buckets with client exports and application logs. Account-level Block Public Access was not enabled.

**Recommendation:** Enable S3 Block Public Access at the organization level, review and correct bucket policies and ACLs, and check access logs for unauthorized reads. Add an AWS Config rule with auto-remediation and notify affected clients as contracts require.

- **Control reference (as logged):** CIS Controls v8 3.3 (Configure Data Access Control Lists)
- **Assets:** [AST-002](01-asset-inventory.md#ast-002)
- **Risks:** [RSK-001](03-risk-register.md#rsk-001)
- **Controls:** [CTL-001](04-control-catalog.md#ctl-001)

### <a id="fnd-002"></a>**FND-002** — Overly permissive IAM policies with wildcard permissions

**Description:** Over 30 customer-managed IAM policies grant full actions or resources through wildcards, and several engineer roles can modify IAM itself. This allows privilege escalation across client accounts.

**Recommendation:** Replace wildcard policies with least-privilege roles using IAM Access Analyzer findings, remove IAM write access from non-administrator roles, and add permission boundaries. Review role assignments every quarter.

- **Control reference (as logged):** CIS Controls v8 6.8 (Define and Maintain Role-Based Access Control)
- **Assets:** [AST-001](01-asset-inventory.md#ast-001)
- **Risks:** [RSK-002](03-risk-register.md#rsk-002)
- **Controls:** [CTL-002](04-control-catalog.md#ctl-002), [CTL-007](04-control-catalog.md#ctl-007)

### <a id="fnd-003"></a>**FND-003** — Multi-factor authentication not enforced for all console users

**Description:** 23 of 140 IAM users with console access have no MFA device, including 4 with administrative permissions. Root account MFA was confirmed as enabled.

**Recommendation:** Enforce MFA through an IAM policy condition, migrate console access to IAM Identity Center with SSO, and disable IAM users who do not enroll by the deadline.

- **Control reference (as logged):** CIS Controls v8 6.5 (Require MFA for Administrative Access)
- **Assets:** [AST-001](01-asset-inventory.md#ast-001)
- **Risks:** [RSK-002](03-risk-register.md#rsk-002)
- **Controls:** [CTL-003](04-control-catalog.md#ctl-003)

### <a id="fnd-004"></a>**FND-004** — Security groups allow SSH and RDP from any address

**Description:** AWS Config flagged 17 security groups across production and staging that allow inbound SSH or RDP from 0.0.0.0/0. Two attach to instances that hold client workloads.

**Recommendation:** Remove open administrative rules, require access through AWS Systems Manager Session Manager or a controlled bastion, and add a detective rule that alerts on new open administrative ports.

- **Control reference (as logged):** CIS Controls v8 4.4 (Implement and Manage a Firewall on Servers)
- **Assets:** [AST-004](01-asset-inventory.md#ast-004)
- **Risks:** [RSK-003](03-risk-register.md#rsk-003)
- **Controls:** [CTL-006](04-control-catalog.md#ctl-006), [CTL-008](04-control-catalog.md#ctl-008)

### <a id="fnd-005"></a>**FND-005** — Unencrypted EBS volumes on internal tooling instances

**Description:** Self-assessment found 12 EBS volumes on internal tooling instances without encryption. None held client data, but they stored configuration and credentials files.

**Recommendation:** Completed: volumes were re-created with KMS encryption and default EBS encryption was enabled in every region. Closure was confirmed by a follow-up configuration scan.

- **Control reference (as logged):** CIS Controls v8 3.11 (Encrypt Sensitive Data at Rest)
- **Assets:** —
- **Risks:** [RSK-004](03-risk-register.md#rsk-004)
- **Controls:** [CTL-005](04-control-catalog.md#ctl-005)

---
[← Asset Inventory](01-asset-inventory.md) · Next: [Risk Register →](03-risk-register.md)

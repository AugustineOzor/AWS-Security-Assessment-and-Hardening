# 03 · Risk Register

**Navigate:** [README](../README.md) · [Assets](01-asset-inventory.md) · [Findings](02-audit-findings.md) · [Risks](03-risk-register.md) · [Controls](04-control-catalog.md)

Risks written as threat, vulnerability, impact and asset. Score is Likelihood × Impact; bands are Low 1–4, Medium 5–9, High 10–16, Critical 17–25.

## Summary

| ID | Risk | Category | Status | Likelihood | Impact | Score | Owner |
|---|---|---|---|---|---|---|---|
| [RSK-001](#rsk-001) | External attacker exploits publicly accessible S3 bucket settings causing disclosure of client data in Client Data S3 Buckets | Data Protection | In Progress | 4 - Likely | 5 - Severe | 20 Critical | Cloud Platform Manager |
| [RSK-002](#rsk-002) | Compromised engineer account exploits overly permissive IAM policies causing privilege escalation across client AWS accounts | Access Control | Open | 3 - Possible | 5 - Severe | 15 High | Cloud Security Lead |
| [RSK-003](#rsk-003) | Engineer misconfiguration exploits missing infrastructure guardrails causing public exposure or outage in production AWS environments | Human Error | Open | 4 - Likely | 4 - Major | 16 High | DevOps Lead |
| [RSK-004](#rsk-004) | Attacker with snapshot access exploits unencrypted EBS volumes causing exposure of credentials and configuration on internal tooling instances | Data Protection | Closed | 2 - Unlikely | 3 - Moderate | 6 Medium | Infrastructure Manager |

## Details

### <a id="rsk-001"></a>**RSK-001** — External attacker exploits publicly accessible S3 bucket settings causing disclosure of client data in Client Data S3 Buckets

**Description:** Audit found 9 buckets holding client exports and logs open to the internet, with account-level Block Public Access disabled. Linked to finding FND-001.

**Decision rationale:** Likely because public buckets are continuously scanned by attackers and logs may already show unauthorized reads. Severe because the data belongs to 50+ enterprise clients, with contractual, regulatory and reputational consequences.

- **Findings:** [FND-001](02-audit-findings.md#fnd-001)
- **Assets:** [AST-002](01-asset-inventory.md#ast-002)
- **Controls:** [CTL-001](04-control-catalog.md#ctl-001), [CTL-008](04-control-catalog.md#ctl-008), [CTL-010](04-control-catalog.md#ctl-010)

### <a id="rsk-002"></a>**RSK-002** — Compromised engineer account exploits overly permissive IAM policies causing privilege escalation across client AWS accounts

**Description:** Wildcard IAM policies and roles that can edit IAM let one stolen credential reach many accounts. 23 console users also lack MFA. Linked to findings FND-002 and FND-003.

**Decision rationale:** Possible because credential theft requires an initial foothold, but MFA gaps raise the odds. Severe because the management and identity layer controls access to every client environment.

- **Findings:** [FND-002](02-audit-findings.md#fnd-002), [FND-003](02-audit-findings.md#fnd-003)
- **Assets:** [AST-001](01-asset-inventory.md#ast-001)
- **Controls:** [CTL-002](04-control-catalog.md#ctl-002), [CTL-003](04-control-catalog.md#ctl-003), [CTL-007](04-control-catalog.md#ctl-007)

### <a id="rsk-003"></a>**RSK-003** — Engineer misconfiguration exploits missing infrastructure guardrails causing public exposure or outage in production AWS environments

**Description:** Changes are made through the console and ad hoc scripts without policy-as-code checks, so risky settings such as open security groups reach production. Linked to finding FND-004.

**Decision rationale:** Likely because the audit already found several misconfigurations created this way, and 17 security groups were open to any address. Major rather than severe because most mistakes are caught within days, but a client-facing outage or exposure would breach SLAs.

- **Findings:** [FND-004](02-audit-findings.md#fnd-004)
- **Assets:** [AST-004](01-asset-inventory.md#ast-004)
- **Controls:** [CTL-006](04-control-catalog.md#ctl-006), [CTL-008](04-control-catalog.md#ctl-008)

### <a id="rsk-004"></a>**RSK-004** — Attacker with snapshot access exploits unencrypted EBS volumes causing exposure of credentials and configuration on internal tooling instances

**Description:** 12 unencrypted EBS volumes on internal tooling held configuration and credential files. Volumes were re-created with KMS encryption and default encryption enabled. Linked to finding FND-005.

**Decision rationale:** Residual risk rated unlikely because encryption is now enforced by default in every region and verified by a follow-up scan. Moderate impact because the data was internal, not client data.

- **Findings:** [FND-005](02-audit-findings.md#fnd-005)
- **Assets:** —
- **Controls:** [CTL-005](04-control-catalog.md#ctl-005)

---
[← Audit Findings](02-audit-findings.md) · Next: [Control Catalog →](04-control-catalog.md)

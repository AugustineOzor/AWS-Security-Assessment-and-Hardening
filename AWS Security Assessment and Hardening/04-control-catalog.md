# 04 · Control Catalog

**Navigate:** [README](../README.md) · [Assets](01-asset-inventory.md) · [Findings](02-audit-findings.md) · [Risks](03-risk-register.md) · [Controls](04-control-catalog.md)

Hardening controls with effectiveness ratings. References are CIS Controls v8 safeguard numbers and NIST CSF 2.0 subcategory IDs; they are paraphrased pointers, so verify against the source text.

## Summary

| ID | Name | Framework | Reference | Effectiveness | Frequency | Owner |
|---|---|---|---|---|---|---|
| [CTL-001](#ctl-001) | S3 Block Public Access enforced organization-wide | CIS Controls | 3.3 | Ineffective | Continuous | Cloud Platform Manager |
| [CTL-002](#ctl-002) | Least-privilege IAM roles with permission boundaries | CIS Controls | 6.8 | Partially Effective | Continuous | Cloud Security Lead |
| [CTL-003](#ctl-003) | MFA enforced for all console users through IAM Identity Center | CIS Controls | 6.5 | Partially Effective | Continuous | Cloud Security Lead |
| [CTL-004](#ctl-004) | Organization-wide CloudTrail with log file validation | CIS Controls | 8.2 | Effective | Continuous | Security Operations Lead |
| [CTL-005](#ctl-005) | Default KMS encryption for EBS, S3 and RDS | CIS Controls | 3.11 | Effective | Quarterly | Infrastructure Manager |
| [CTL-006](#ctl-006) | Security group change monitoring with AWS Config rules | CIS Controls | 4.4 | Partially Effective | Continuous | Network Security Lead |
| [CTL-007](#ctl-007) | Quarterly IAM access reviews | NIST CSF | PR.AA-05 | Not Tested | Quarterly | Cloud Security Lead |
| [CTL-008](#ctl-008) | Infrastructure-as-code policy checks in CI/CD | NIST CSF | PR.PS-01 | Partially Effective | Continuous | DevOps Lead |
| [CTL-009](#ctl-009) | Automated cross-region backups with restore testing | CIS Controls | 11.2 | Not Tested | Quarterly | Infrastructure Manager |
| [CTL-010](#ctl-010) | Cloud exposure incident response and client notification runbook | NIST CSF | RS.MA-01 | Not Tested | Annual | Security Operations Lead |

## Details

### <a id="ctl-001"></a>**CTL-001** — S3 Block Public Access enforced organization-wide

**Description:** Enable S3 Block Public Access at the AWS Organizations level and deny its removal with a service control policy. Target: zero publicly readable buckets in any client account, verified daily by an AWS Config rule.

**Decision rationale:** The audit found 9 public buckets and the account-level setting was off, so the control was not operating. Rating moves up after enforcement and a clean daily scan. Linked to FND-001 and RSK-001.

- **Findings:** [FND-001](02-audit-findings.md#fnd-001)
- **Risks:** [RSK-001](03-risk-register.md#rsk-001)
- **Assets:** —

### <a id="ctl-002"></a>**CTL-002** — Least-privilege IAM roles with permission boundaries

**Description:** Grant access only through role-based IAM roles built from IAM Access Analyzer findings, with permission boundaries that block IAM write actions for non-administrators. Target: no customer-managed policy with wildcard actions and resources.

**Decision rationale:** Boundaries apply to new roles only, and over 30 existing policies still use wildcards. Design is sound but coverage is incomplete. Linked to FND-002 and RSK-002.

- **Findings:** [FND-002](02-audit-findings.md#fnd-002)
- **Risks:** [RSK-002](03-risk-register.md#rsk-002)
- **Assets:** —

### <a id="ctl-003"></a>**CTL-003** — MFA enforced for all console users through IAM Identity Center

**Description:** Require MFA for every human console user through SSO, with hardware keys for administrators, and disable IAM users that have not enrolled. Target: 100% of console users enrolled, reported weekly.

**Decision rationale:** 117 of 140 users are enrolled (about 84%) and 4 administrators still lack MFA. Control works where deployed but has gaps. Linked to FND-003.

- **Findings:** [FND-003](02-audit-findings.md#fnd-003)
- **Risks:** [RSK-002](03-risk-register.md#rsk-002)
- **Assets:** —

### <a id="ctl-004"></a>**CTL-004** — Organization-wide CloudTrail with log file validation

**Description:** Run a multi-region organization trail delivering to a write-protected central log account, with log file validation on and 12-month retention. Alert if logging is stopped in any account.

**Decision rationale:** Sampled 10 accounts and found logging active in all regions with validation enabled. Stop-logging alerts were tested in staging and fired within 5 minutes.

- **Findings:** —
- **Risks:** —
- **Assets:** [AST-001](01-asset-inventory.md#ast-001)

### <a id="ctl-005"></a>**CTL-005** — Default KMS encryption for EBS, S3 and RDS

**Description:** Enable default encryption for EBS in every region and require KMS encryption for S3 buckets and RDS instances. Review key policies and rotation every quarter. Target: zero unencrypted volumes or databases.

**Decision rationale:** Follow-up configuration scan found no unencrypted resources after FND-005 was closed. Quarterly key policy reviews are documented.

- **Findings:** [FND-005](02-audit-findings.md#fnd-005)
- **Risks:** [RSK-004](03-risk-register.md#rsk-004)
- **Assets:** [AST-002](01-asset-inventory.md#ast-002), [AST-003](01-asset-inventory.md#ast-003)

### <a id="ctl-006"></a>**CTL-006** — Security group change monitoring with AWS Config rules

**Description:** Alert within 15 minutes when a security group allows SSH or RDP from 0.0.0.0/0, and open a ticket for the resource owner. Target: no open administrative ports in production.

**Decision rationale:** Detection works and flagged 17 open groups, but there is no automatic remediation and tickets were closed slowly. Detective only, not preventive. Linked to FND-004.

- **Findings:** [FND-004](02-audit-findings.md#fnd-004)
- **Risks:** [RSK-003](03-risk-register.md#rsk-003)
- **Assets:** —

### <a id="ctl-007"></a>**CTL-007** — Quarterly IAM access reviews

**Description:** Resource owners review every role, group and cross-account trust each quarter and revoke unneeded access within 10 business days. Target: 100% of reviews completed on time with evidence retained.

**Decision rationale:** The process is defined but the first review cycle has not run, so there is no evidence to test. Linked to RSK-002.

- **Findings:** [FND-002](02-audit-findings.md#fnd-002)
- **Risks:** [RSK-002](03-risk-register.md#rsk-002)
- **Assets:** —

### <a id="ctl-008"></a>**CTL-008** — Infrastructure-as-code policy checks in CI/CD

**Description:** Scan Terraform and CloudFormation templates with policy-as-code rules before deployment and block merges that create public storage, open administrative ports or unencrypted data stores. Target: all production changes pass through the pipeline.

**Decision rationale:** Checks run for staging and about half of production repositories. Console changes still bypass the pipeline. Linked to RSK-003.

- **Findings:** [FND-004](02-audit-findings.md#fnd-004)
- **Risks:** [RSK-001](03-risk-register.md#rsk-001), [RSK-003](03-risk-register.md#rsk-003)
- **Assets:** [AST-005](01-asset-inventory.md#ast-005)

### <a id="ctl-009"></a>**CTL-009** — Automated cross-region backups with restore testing

**Description:** Back up client databases and storage daily to the cross-region vault and complete a documented restore test each quarter. Target: restore within the agreed recovery time objective.

**Decision rationale:** Backup jobs report success, but no restore test has been performed, so recoverability is unproven. Related asset: AST-006.

- **Findings:** —
- **Risks:** —
- **Assets:** [AST-003](01-asset-inventory.md#ast-003), [AST-006](01-asset-inventory.md#ast-006)

### <a id="ctl-010"></a>**CTL-010** — Cloud exposure incident response and client notification runbook

**Description:** Maintain a runbook for exposed storage, leaked credentials and compromised accounts, including client notification steps and contractual deadlines. Exercise it once a year. Target: clients notified within the contract window.

**Decision rationale:** The runbook was drafted after the audit and has not been exercised. Rating stays Not Tested until a tabletop is completed.

- **Findings:** —
- **Risks:** [RSK-001](03-risk-register.md#rsk-001)
- **Assets:** —

---
[← Risk Register](03-risk-register.md) · [Back to README](../README.md)

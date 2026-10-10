# 01 · Asset Inventory

**Navigate:** [README](../README.md) · [Assets](01-asset-inventory.md) · [Findings](02-audit-findings.md) · [Risks](03-risk-register.md) · [Controls](04-control-catalog.md)

AWS accounts, data stores and network components in scope for the CIS AWS Benchmark assessment.

## Summary

| ID | Name | Type | Classification | Owner | Environment |
|---|---|---|---|---|---|
| [AST-001](#ast-001) | AWS Organizations Management and Identity Center | Cloud Account | Critical | Cloud Security Lead | Production |
| [AST-002](#ast-002) | Client Data S3 Buckets | Data Store | Critical | Cloud Platform Manager | Production |
| [AST-003](#ast-003) | Aurora PostgreSQL Client Databases | Database | High | Database Engineering Lead | Production |
| [AST-004](#ast-004) | VPC Transit Gateway and Perimeter Firewalls | Network Device | High | Network Security Lead | Production |
| [AST-005](#ast-005) | Staging EKS Cluster | Application | Medium | DevOps Lead | Staging |
| [AST-006](#ast-006) | Cross-Region Backup Vault | Data Store | High | Infrastructure Manager | DR |

## Details

### <a id="ast-001"></a>**AST-001** — AWS Organizations Management and Identity Center

Management account, SSO and IAM roles governing access to all client and internal AWS accounts. Audit found overly permissive IAM policies; compromise would expose every client environment.

- **Findings:** [FND-002](02-audit-findings.md#fnd-002), [FND-003](02-audit-findings.md#fnd-003)
- **Risks:** [RSK-002](03-risk-register.md#rsk-002)
- **Controls:** [CTL-004](04-control-catalog.md#ctl-004)

### <a id="ast-002"></a>**AST-002** — Client Data S3 Buckets

S3 buckets holding enterprise client data, exports and application logs for 50+ clients. Audit found publicly accessible buckets; Block Public Access and bucket policy review are in scope.

- **Findings:** [FND-001](02-audit-findings.md#fnd-001)
- **Risks:** [RSK-001](03-risk-register.md#rsk-001)
- **Controls:** [CTL-005](04-control-catalog.md#ctl-005)

### <a id="ast-003"></a>**AST-003** — Aurora PostgreSQL Client Databases

Managed relational databases serving client applications, in private subnets with encryption at rest. Snapshot sharing, encryption and access settings are checked against the CIS AWS Benchmark.

- **Findings:** —
- **Risks:** —
- **Controls:** [CTL-005](04-control-catalog.md#ctl-005), [CTL-009](04-control-catalog.md#ctl-009)

### <a id="ast-004"></a>**AST-004** — VPC Transit Gateway and Perimeter Firewalls

Hub-and-spoke network connecting client VPCs, with security groups and AWS Network Firewall at the perimeter. Overly open security group rules are a known audit concern.

- **Findings:** [FND-004](02-audit-findings.md#fnd-004)
- **Risks:** [RSK-003](03-risk-register.md#rsk-003)
- **Controls:** —

### <a id="ast-005"></a>**AST-005** — Staging EKS Cluster

Kubernetes cluster used to test client application releases before production. Uses synthetic data only, but shares the CI/CD pipeline and IAM trust paths with production.

- **Findings:** —
- **Risks:** —
- **Controls:** [CTL-008](04-control-catalog.md#ctl-008)

### <a id="ast-006"></a>**AST-006** — Cross-Region Backup Vault

AWS Backup vault replicating encrypted snapshots to a secondary region for disaster recovery. Holds copies of client data and must meet recovery time objectives.

- **Findings:** —
- **Risks:** —
- **Controls:** [CTL-009](04-control-catalog.md#ctl-009)

---
Next: [Audit Findings →](02-audit-findings.md)

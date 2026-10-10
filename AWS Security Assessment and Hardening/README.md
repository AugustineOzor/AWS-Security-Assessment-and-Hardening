<p align="center">
  <img src="assets/banner.jpg" alt="CloudNative Corp AWS Security Assessment and Hardening Roadmap" width="100%">
</p>

# AWS Security Assessment & Hardening Roadmap

A fictional GRC portfolio project showing how a Cloud Security Engineer assesses an AWS estate against the CIS AWS Benchmark and builds a hardening roadmap. CloudNative Corp runs AWS infrastructure for 50+ enterprise clients, manages over $15M in annual cloud spend, and has 400 employees. A recent audit found public S3 buckets and overly permissive IAM policies.

## Contents

| # | Document | What it covers |
|---|---|---|
| 1 | [Asset Inventory](docs/01-asset-inventory.md) | 6 AWS assets in scope, with classification, owner and environment |
| 2 | [Audit Findings](docs/02-audit-findings.md) | 5 findings with severity, status, owner, due date and recommendation |
| 3 | [Risk Register](docs/03-risk-register.md) | 4 risks written as threat, vulnerability, impact and asset, with scores and rationale |
| 4 | [Control Catalog](docs/04-control-catalog.md) | 10 hardening controls with framework references, effectiveness ratings and rationale |

## How the documents connect

Every record has an anchor, and each document links to the others:

- **Findings** point to the assets affected, the risk they create and the controls that address them.
- **Risks** point to the findings behind them, the assets exposed and the controls that reduce them.
- **Controls** point to the findings and risks they remediate and the assets they protect.
- **Assets** list the findings, risks and controls that apply to them.

The cross-references are a portfolio mapping created for this case study.

## Frameworks referenced

CIS Controls v8 · NIST CSF 2.0

## Disclaimer

All organizations, assets, findings, ratings and dates are fictional and were created for skills practice and portfolio use. This is not a formal audit or compliance determination. Framework references are paraphrased pointers written from memory, so verify them against the source text before relying on them.

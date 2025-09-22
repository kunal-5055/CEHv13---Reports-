# Module 19 — Cloud Computing Hacking ☁️🔐

> **Goal:** Understand cloud attack surfaces across IaaS, PaaS, and SaaS, perform safe security assessments in cloud environments, and produce remediation guidance tailored for cloud-native architectures.

---

## Table of contents

1. [Overview](#overview)
2. [Learning objectives](#learning-objectives)
3. [Prerequisites](#prerequisites)
4. [Cloud platforms & services covered](#cloud-platforms--services-covered)
5. [Lab environment & tools](#lab-environment--tools)
6. [Course outline & lessons](#course-outline--lessons)
7. [Hands-on labs / exercises](#hands-on-labs--exercises)
8. [Deliverables & assessment](#deliverables--assessment)
9. [Cheat sheet / quick commands](#cheat-sheet--quick-commands)
10. [Safety, ethics & legal](#safety-ethics--legal)
11. [Suggested further learning & resources](#suggested-further-learning--resources)
12. [Instructor notes (for maintainers)](#instructor-notes-for-maintainers)

---

## Overview

Cloud platforms introduce new architectures, shared responsibility models, and automation that change how attacks are mounted and how defenses operate. This module covers discovery, misconfiguration hunting, identity and access management (IAM) abuse, lateral movement in cloud networks, serverless risks, container/cloud-native vulnerabilities, and secure remediation strategies.

---

## Learning objectives

By the end of Module 19 you will be able to:

* 🔍 Enumerate cloud resources and identify misconfigurations across major providers.
* 🛂 Audit and exploit weak IAM policies and credentials safely in lab environments.
* 🌐 Map cloud network and perform lateral movement using cloud-native features (metadata APIs, instance profiles, service accounts).
* 🧩 Test container & serverless deployments for common weaknesses (image vulnerabilities, insecure function code, overly-permissive roles).
* 🛠 Recommend cloud-specific remediation: least privilege IAM, secure CI/CD pipelines, secret management, and monitoring.

---

## Prerequisites

* Comfortable with Linux, networking, and basic web application testing.
* Familiarity with AWS/GCP/Azure concepts (VMs, storage, IAM, VPCs) is helpful but not required.
* Basic scripting (Python/bash) for automation during labs.

---

## Cloud platforms & services covered

* **AWS**: EC2, S3, IAM, Lambda, ECS/EKS, CloudFormation, IAM Roles, Metadata Service
* **Azure**: VMs, Blob Storage, Managed Identities, Functions, AKS, ARM templates
* **GCP**: Compute Engine, Cloud Storage, IAM, Cloud Functions, GKE, Service Accounts

Note: labs use isolated, disposable accounts/projects with limited resources and billing controls.

---

## Lab environment & tools

**Lab setup recommendations:**

* Create separate cloud accounts/projects per student or use pre-provisioned lab tenancy. Enable billing limits and alerts.
* Use Terraform/CloudFormation/ARM templates to provision intentionally vulnerable setups (CSP-Playground-style). Destroy after labs.

**Tools (suggested):**

* `awscli`, `gcloud`, `az` — official CLIs
* `kubectl`, `helm` — for Kubernetes labs
* `pacu` — AWS exploitation framework (lab-only)
* `CloudGoat`, `csp-generator`, `csp-playground` — vulnerable lab environments
* `Pacu`/`GCPri`/`azurite` equivalents for cloud-specific exploitation
* `tfsec`, `checkov`, `kics` — IaC scanning
* `curl`, `jq`, `httpie` — API interactions
* `kubesec`, `kube-hunter`, `trivy` — container & cluster scanning
* `mitmproxy`, `Burp Suite` — intercepting web traffic
* `HashiCorp Vault` / AWS Secrets Manager / Azure Key Vault (for remediation labs)

---

## Course outline & lessons

1. **Cloud security fundamentals** — shared responsibility, cloud asset inventory, billing considerations.
2. **Discovery & asset inventory** — enumerating accounts, S3/GCS/Azure Blob buckets, public resources, misconfigured storage.
3. **IAM & identity attacks** — role chaining, credential leakage, abusing instance metadata/service accounts.
4. **Network & perimeter** — VPC/VNet architecture, security groups, NSGs, misconfigured peering, and firewall rules.
5. **Serverless security** — insecure code, event injection, improper permissions for functions.
6. **Kubernetes & containers** — image supply chain, insecure manifests, RBAC misconfigurations.
7. **Infrastructure-as-Code (IaC) security** — scanning templates, preventing drift, secure pipelines.
8. **Lateral movement & persistence** — abusing roles, service accounts, scheduled functions, and resource policies.
9. **Detection & mitigation** — logging, alerting, EDR solutions, least privilege, automated remediation.
10. **Incident response in cloud** — containment, forensic data acquisition, and recovery playbooks.

---

## Hands-on labs / exercises (suggested)

Each lab should include objective, step-by-step actions, commands used, outputs/screenshots, and remediation recommendations.

### Lab A — Cloud Recon & Public Resource Discovery 🔎

* Enumerate public S3/GCS/Azure blobs and identify publicly accessible buckets/blobs.
* Use `aws s3 ls` and `gcloud`/`az` equivalents and `shodan` for public endpoints.

### Lab B — IAM Misconfigurations & Role Escalation 🛂

* Find overly permissive policies and practice lateral privilege escalation using instance profiles or service accounts (lab-only).
* Use `aws sts get-caller-identity`, `curl http://169.254.169.254/latest/meta-data/iam/security-credentials/`, and PACU modules against lab accounts.

### Lab C — Serverless Function Assessment ⚡

* Inspect deployed functions for secrets in environment variables, vulnerable dependencies, and excessive roles.
* Deploy a vulnerable Lambda/Function and exploit via crafted events or API gateway.

### Lab D — Container & Kubernetes Assessment 🐳

* Scan images with `trivy`, inspect manifests for `hostPath`, `privileged`, `hostNetwork`, and test RBAC misconfigurations with `kube-hunter`.
* Practice escaping container if lab container intentionally vulnerable (non-destructive and controlled).

### Lab E — IaC Scanning & Secure Pipelines 🔁

* Run `tfsec`/`checkov` on Terraform/ARM/CloudFormation templates.
* Build a GitHub Actions/CI pipeline that scans IaC and blocks unsafe merges.

### Lab F — Incident Response Simulation 🚨

* Simulate data exfiltration from S3 and practice containment: revoke keys, remove role bindings, enable logging, restore from backups.

---

## Example repo structure

```
module-19-cloud-hacking/
├─ README.md                ← this file
├─ labs/
│  ├─ lab-a-recon/
│  ├─ lab-b-iam/
│  ├─ lab-c-serverless/
│  ├─ lab-d-k8s/
│  └─ lab-e-iac/
├─ templates/               ← terraform/arm/cloudformation vulnerable templates
├─ scripts/                 ← helper scripts (scanners, exploit PoCs)
└─ reports/                 ← lab reports & remediation docs
```

---

## Deliverables & assessment

For each lab submit:

* Lab report (concise — max 2 pages) with objective, steps, commands, screenshots, and remediation.
* PoC code or commands used (clearly marked non-destructive).
* Recommended remediation checklist and risk rating (e.g., CVSS or cloud-impact score).

Grading: correctness, clarity, non-destructive approach, remediation quality, and understanding of cloud-native concerns.

---

## Cheat sheet / Quick commands

* AWS: list S3 buckets and public objects:
  `aws s3 ls && aws s3api list-buckets --query "Buckets[].Name"`
  `aws s3api get-bucket-acl --bucket example-bucket`
* Get AWS metadata (lab instance):
  `curl http://169.254.169.254/latest/meta-data/`
  `curl http://169.254.169.254/latest/meta-data/iam/security-credentials/`
* IAM policy simulation: `aws iam simulate-principal-policy --policy-source-arn <arn> --action-names s3:PutObject`
* Terraform scanning: `tfsec .`
* Kubernetes manifest checks: `kubeval manifest.yaml && trivy fs .`
* GCP metadata: `curl "http://metadata.google.internal/computeMetadata/v1/" -H "Metadata-Flavor: Google"`

---

## Safety, ethics & legal ⚖️

* Only test in accounts/projects you own or have explicit permission for.
* Use disposable lab accounts, resource limits, and billing alerts to avoid unexpected charges.
* Keep tests non-destructive unless explicitly permitted and documented.
* Follow responsible disclosure for vulnerabilities affecting vendors or other tenants.

---


---


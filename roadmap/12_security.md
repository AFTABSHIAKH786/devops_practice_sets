# 🔒 Security & DevSecOps

> **Goal:** Integrate security thinking and tooling into every layer of the platform — from code commits to production runtime. Security is not a checkbox; it's a practice.
>
> **Target:** 0–1 year → can implement baseline DevSecOps practices across a platform

---

## 📚 Resources

### Primary
- **OWASP DevSecOps Guideline:** https://owasp.org/www-project-devsecops-guideline/
- **CNCF Security Whitepaper:** https://github.com/cncf/tag-security/blob/main/security-whitepaper/v2/CNCF_cloud-native-security-whitepaper-May2022-v2.pdf
- **NSA/CISA Kubernetes Hardening Guide:** https://media.defense.gov/2022/Aug/29/2003066362/-1/-1/0/CTR_KUBERNETES_HARDENING_GUIDANCE_1.2_20220829.PDF

### Books
- *Container Security* — Liz Rice (free chapter previews on GitHub)
- *The DevOps Handbook* — Gene Kim (includes security culture)
- *Hacking Kubernetes* — Andrew Martin & Michael Hausenblas

### Courses / Videos
- **KodeKloud CKS Course:** https://kodekloud.com/courses/certified-kubernetes-security-specialist/
- **TechWorld with Nana — DevSecOps:** https://youtu.be/3RiLGsVRb3o
- **freeCodeCamp — Ethical Hacking:** https://youtu.be/3Kq1MIfTWCE

---

## Stage 0 — DevSecOps Mindset & Threat Modeling

### Learn
- Shift-left security: find issues at commit, not in production
- STRIDE threat model: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege
- Attack surface: what can an attacker reach?
- Defense in depth: multiple security layers
- The principle of least privilege (PoLP)
- Zero trust: never trust, always verify

### Resources
- https://owasp.org/www-community/Threat_Modeling
- **STRIDE explained:** https://www.microsoft.com/en-us/security/blog/2007/09/11/stride-chart/

### 🟢 Easy Exercises
1. Perform a threat model for your Docker-based web app using STRIDE — list one threat per category
2. Identify the attack surface of a Kubernetes cluster: list every entry point an attacker could use
3. Write the "defense in depth" layers for a K8s-hosted app: what controls exist at each layer?

---

## Stage 1 — Secrets Management

### Learn
- Why environment variables are NOT secure for secrets
- Kubernetes Secrets: base64 (not encrypted at rest by default)
- HashiCorp Vault: dynamic secrets, leases, policies
- AWS Secrets Manager and SSM Parameter Store
- SOPS + age/KMS for secrets-in-Git
- External Secrets Operator (ESO): sync secrets from Vault/AWS to K8s
- Sealed Secrets: encrypts K8s secrets for safe Git storage

### Resources
- **HashiCorp Vault docs:** https://developer.hashicorp.com/vault/docs
- **Vault getting started:** https://developer.hashicorp.com/vault/tutorials/getting-started
- **SOPS:** https://github.com/mozilla/sops
- **External Secrets Operator:** https://external-secrets.io/

### 🟢 Easy Exercises
1. Run Vault in dev mode with Docker, write a secret, read it back, revoke it
2. Install Sealed Secrets on minikube, seal a Kubernetes Secret, commit the sealed version to Git, apply it, and verify the original secret is created
3. Use SOPS with an age key to encrypt a YAML file — commit the encrypted version to Git

### 🟡 Medium Exercises
1. Configure Vault with the Kubernetes auth method so pods can authenticate to Vault using their ServiceAccount token
2. Install External Secrets Operator, configure an `ExternalSecret` that syncs from AWS Secrets Manager to a Kubernetes Secret
3. Write a Vault policy that allows a specific app to read only its own secrets path

### 🔴 Hard Exercises
1. Implement Vault agent injection: vault-agent-injector auto-populates a secret file into a pod at runtime without modifying the app
2. Design a secret rotation workflow: Vault generates a new DB password, the DB role is updated, and app pods are restarted rolling-style

### 🏗 DevOps Project
Implement a production secrets management system: Vault on Kubernetes with ESO syncing secrets to namespaces, SOPS-encrypted secrets in Git for non-sensitive config, and a rotation runbook for DB credentials.

---

## Stage 2 — Container & Image Security

### Learn
- Image vulnerability scanning: CVEs, severity levels
- Trivy, Grype, Snyk, Docker Scout
- Base image hygiene: minimal images, non-root users, read-only filesystems
- Image signing: cosign, Docker Content Trust (Notary)
- SBOM (Software Bill of Materials): syft, Grype
- Admission controllers: OPA Gatekeeper, Kyverno for image policies
- Runtime security: Falco

### Resources
- **Trivy:** https://github.com/aquasecurity/trivy
- **cosign:** https://github.com/sigstore/cosign
- **Falco:** https://falco.org/docs/
- **Kyverno:** https://kyverno.io/

### 🟢 Easy Exercises
1. Scan 5 Docker images with Trivy — rank them by number of critical CVEs: `nginx:latest`, `nginx:alpine`, `ubuntu:latest`, `alpine:latest`, `python:3.12-alpine`
2. Generate an SBOM for your app image using `syft` and inspect what packages are in it
3. Run `docker scout cves myimage` and interpret the output

### 🟡 Medium Exercises
1. Install Falco on a Kubernetes cluster, trigger a rule violation (write to `/etc` in a container), and see the alert
2. Install Kyverno, write a policy that blocks any pod running as root (UID 0)
3. Use cosign to sign a Docker image and configure Kyverno to verify the signature before allowing the image to run

### 🔴 Hard Exercises
1. Build a CI/CD pipeline step that: scans with Trivy, fails on CRITICAL CVEs, generates and uploads an SBOM, and signs the image with cosign — all before pushing to the registry
2. Write a Kyverno ClusterPolicy that: blocks `latest` image tags, requires all images to come from your private registry, and requires a specific label on every Deployment

---

## Stage 3 — Kubernetes Security

### Learn
- Pod Security Standards (privileged, baseline, restricted)
- RBAC: least privilege for every ServiceAccount
- Network Policies: default-deny, allow only necessary traffic
- Seccomp profiles
- AppArmor profiles
- API Server audit logging
- CIS Kubernetes Benchmark

### Resources
- https://kubernetes.io/docs/concepts/security/
- **kube-bench (CIS benchmark scanner):** https://github.com/aquasecurity/kube-bench
- **kube-hunter (penetration testing):** https://github.com/aquasecurity/kube-hunter

### 🟢 Easy Exercises
1. Run `kube-bench` on your cluster — identify the top 5 failing checks and fix at least 2
2. Create a Namespace with the `restricted` Pod Security Standard enforced — verify that a root-running pod is rejected
3. Write a NetworkPolicy: default-deny all ingress in a namespace, then allow only traffic from pods with label `app=frontend`

### 🟡 Medium Exercises
1. Implement RBAC for a CI/CD service account: can only create/update Deployments in `staging` namespace, read-only everywhere else
2. Enable API server audit logging and write a query that finds all `exec` actions in the last hour

### 🔴 Hard Exercises
1. Run `kube-hunter` in active mode against your cluster — report all findings and fix critical ones
2. Implement a complete multi-tenant security model: separate namespaces, RBAC, NetworkPolicy, and ResourceQuota for 3 teams

---

## Stage 4 — Supply Chain Security

### Learn
- Software supply chain attacks (SolarWinds, Log4Shell context)
- SLSA (Supply-chain Levels for Software Artifacts)
- Sigstore: cosign, Rekor (transparency log), Fulcio (CA)
- SBOM formats: SPDX, CycloneDX
- Dependency scanning: Dependabot, Renovate
- Provenance attestations

### Resources
- **SLSA framework:** https://slsa.dev/
- **Sigstore:** https://www.sigstore.dev/
- **Renovate bot:** https://docs.renovatebot.com/

### 🟢 Easy Exercises
1. Enable Dependabot on a GitHub repo — review and merge the first 3 automated dependency update PRs
2. Generate a CycloneDX SBOM for your Python app and check it for known vulnerabilities with `grype sbom:cyclonedx.json`
3. Explain SLSA Level 1, 2, and 3 in your own words — what additional guarantees does each add?

### 🟡 Medium Exercises
1. Set up Renovate bot on a repository to automatically create PRs for dependency updates
2. Implement SLSA Level 1 for your GitHub Actions pipeline: output a provenance attestation with `slsa-github-generator`

---

## Stage 5 — Compliance & Auditing

### Learn
- CIS Benchmarks: what they are, how to use them
- PCI-DSS, SOC 2, ISO 27001 basics (what they require from a DevOps perspective)
- Policy-as-code: OPA (Open Policy Agent), Conftest
- Audit trails: who did what and when
- Security information and event management (SIEM basics)

### Resources
- **CIS Benchmarks:** https://www.cisecurity.org/cis-benchmarks/
- **OPA docs:** https://www.openpolicyagent.org/docs/latest/
- **Conftest:** https://www.conftest.dev/

### 🟢 Easy Exercises
1. Write an OPA/Rego policy that rejects Kubernetes Deployments with no resource limits set
2. Run Conftest against your Terraform plan output to check for security policy violations
3. List the top 5 audit log events you'd want to alert on for a Kubernetes cluster

### 🟡 Medium Exercises
1. Write a full Rego policy file that enforces: no root containers, no `latest` tags, required labels, and minimum replicas ≥ 2
2. Set up Falco to alert on: shell spawned in a container, file write in /etc, and outbound connection from a sensitive pod

### 🏗 Capstone Project
Implement a full DevSecOps pipeline:
- **Shift left:** gitleaks pre-commit + Trivy in CI + Snyk for dependencies
- **Registry:** cosign image signing + SBOM generation + Kyverno signature verification
- **Runtime:** Falco for anomaly detection + OPA Gatekeeper for admission control
- **Audit:** Kubernetes audit logging + alerts for critical security events

---

## 🔗 Additional Security Resources

### Tools
- **Trivy:** https://github.com/aquasecurity/trivy
- **Falco:** https://falco.org/
- **Kyverno:** https://kyverno.io/
- **OPA Gatekeeper:** https://github.com/open-policy-agent/gatekeeper
- **kube-bench:** https://github.com/aquasecurity/kube-bench
- **gitleaks:** https://github.com/gitleaks/gitleaks
- **Vault:** https://developer.hashicorp.com/vault

### Certification
- **CKS (Certified Kubernetes Security Specialist):** https://training.linuxfoundation.org/certification/certified-kubernetes-security-specialist/

---

*Progress: 0/6 stages complete · Est. time to complete: 6–8 weeks at 3 hrs/day*

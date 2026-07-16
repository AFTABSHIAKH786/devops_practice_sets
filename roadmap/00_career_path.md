# 🗺 Career Path — Junior DevOps → Platform Engineer

> **Goal:** Understand exactly what skills, projects, and experiences move you from one level to the next — and what salary you can realistically expect at each stage.

---

## The Ladder

```
Level 1 — Junior DevOps Engineer       (0–1 year)
        ↓
Level 2 — DevOps Engineer              (1–3 years)
        ↓
Level 3 — Senior DevOps / SRE          (3–5 years)
        ↓
Level 4 — Platform Engineer            (4–7 years)
        ↓
Level 5 — Staff / Principal Eng        (7+ years)
```

---

## Level 1 — Junior DevOps Engineer

**Salary:** ₹4–8 LPA (India) · $40–65k (Remote/Global)

### Skills Expected
- Basic Linux navigation and file operations
- Git: clone, add, commit, push, pull, branch, merge
- Docker: run containers, write basic Dockerfiles
- Basic CI/CD: understand pipelines, trigger builds
- Basic scripting: Bash or Python for simple automation
- Understanding of cloud concepts (not hands-on required yet)

### What You're Doing Day-to-Day
- Running and monitoring deployments
- Writing basic Ansible playbooks or shell scripts
- Fixing broken CI/CD pipelines (with guidance)
- Setting up alerts in Grafana/Datadog (copy-paste level)
- Answering on-call tickets with runbooks

### Portfolio Projects (to get hired)
- [ ] A Dockerized application with a CI/CD pipeline (GitHub Actions)
- [ ] A simple monitoring dashboard (Prometheus + Grafana)
- [ ] An automated backup script in Python or Bash
- [ ] A basic Terraform config that provisions EC2 + S3

### Interview Focus
- Linux commands fluency (top 50 commands)
- Explain how Git branching works
- What is Docker and how does it differ from VMs?
- What is CI/CD and why does it matter?
- What happens when you type google.com in a browser?

---

## Level 2 — DevOps Engineer

**Salary:** ₹8–16 LPA (India) · $65–95k (Remote/Global)

### Skills Expected
- Linux: process management, systemd, networking tools, shell scripting
- Docker: multi-stage builds, Compose, networking, volumes, security
- Kubernetes: Deployments, Services, ConfigMaps, RBAC, basic troubleshooting
- IaC: Terraform (resources, state, modules), Ansible (playbooks, roles, vault)
- CI/CD: design and maintain pipelines, understand artifacts and caching
- Cloud: AWS core services (EC2, S3, RDS, VPC, IAM, EKS)
- Monitoring: Prometheus, Grafana, log aggregation, alerting
- Python: scripting, API clients, automation tools
- Incident response: can diagnose and resolve common production issues

### What You're Doing Day-to-Day
- Owning CI/CD pipelines end-to-end
- Managing Kubernetes clusters
- Writing and maintaining Terraform modules
- Building observability dashboards and alerts
- Participating in on-call rotation (primary responder)
- Code reviews for infrastructure changes

### Portfolio Projects (to get promoted/hired)
- [ ] A production-grade Kubernetes deployment with HPA, PDB, RBAC
- [ ] Terraform modules for VPC + EKS + RDS
- [ ] A monitoring stack: Prometheus + Grafana + Loki + alerts
- [ ] A GitOps workflow with ArgoCD
- [ ] A CLI tool in Python or Go for internal operations

### Certifications to Target at This Level
- AWS Solutions Architect Associate
- HashiCorp Terraform Associate
- CKA (stretch goal)

---

## Level 3 — Senior DevOps / SRE

**Salary:** ₹16–30 LPA (India) · $95–130k (Remote/Global)

### Skills Expected
- Deep Kubernetes: custom controllers, admission webhooks, Operators, advanced networking
- Advanced IaC: multi-account/multi-region, Atlantis or Terraform Cloud, policy-as-code
- Security: DevSecOps, secrets rotation, RBAC at scale, image scanning, supply chain
- Observability: SLOs/SLIs/error budgets, distributed tracing, custom exporters
- Go or Python: writing production-grade internal tools and CLIs
- Platform thinking: you design systems, not just maintain them
- Incident command: lead major incidents, write post-mortems
- Mentoring: you pair with juniors and unblock them

### What You're Doing Day-to-Day
- Designing new platform capabilities
- Building self-service tooling for developers
- Leading incident response
- Setting and owning SLOs
- Evaluating new technologies
- Writing RFCs and ADRs

### Portfolio Projects (to reach this level)
- [ ] A Kubernetes Operator (CRD + controller) for a real use case
- [ ] A custom Prometheus exporter for a domain-specific metric
- [ ] A production-ready internal CLI tool published on GitHub
- [ ] A full GitOps platform (Terraform + ArgoCD + Helm + OPA)
- [ ] A post-mortem for a real or simulated incident (5-why format)

### Certifications to Target at This Level
- CKA (must-have)
- CKAD
- AWS DevOps Engineer Professional
- CKS (stretch)

---

## Level 4 — Platform Engineer

**Salary:** ₹25–50 LPA (India) · $120–160k (Remote/Global)

### Skills Expected
- Platform product mindset: treat developers as customers
- Internal developer platforms (IDP): Backstage, Port, custom portals
- Cost engineering: FinOps, resource optimization, chargeback models
- Multi-cloud or hybrid cloud architecture
- Advanced security: zero-trust, mTLS, SPIFFE/SPIRE, Falco
- Capacity planning and performance engineering
- Technical leadership: you define roadmaps, not follow them
- Deep expertise in at least 2 domains (e.g., Kubernetes + IaC, or Observability + Security)

### What You're Doing Day-to-Day
- Defining and building the internal developer platform
- Setting engineering standards organization-wide
- Evaluating and adopting new tools (with business justification)
- Driving FinOps initiatives
- Mentoring senior engineers
- Partnering with product and security teams

### Portfolio Projects (to reach/demonstrate this level)
- [ ] An internal developer portal (Backstage or similar)
- [ ] A multi-tenant Kubernetes platform with cost allocation
- [ ] A FinOps dashboard showing cluster cost breakdown
- [ ] A platform engineering RFC proposing a major architectural change
- [ ] An open-source contribution to a CNCF project

---

## 🎯 What Actually Gets You Promoted / Hired

### The Hard Truth
Technical skills get you the interview. **Projects and problem-solving get you the offer.**

1. **GitHub Portfolio** — Every project you do should be on GitHub with a clear README, working code, and at minimum a `how-to-run` section.
2. **Writing** — Write about what you're learning. One technical blog post per month (Dev.to, Hashnode, personal blog). This gets you noticed.
3. **Open Source** — Start with issues labeled `good-first-issue` in any CNCF project. Even docs contributions count.
4. **Community** — Join CNCF Slack, local DevOps meetups, cloud provider user groups. Job referrals > cold applications.
5. **Systems Thinking** — At every level above Junior, interviewers want to see that you understand *why* systems are designed a certain way, not just *how* to configure them.

---

## 📅 18-Month Plan (from 0–1 year experience)

### Months 1–3: Foundation
| Week | Focus |
|---|---|
| 1–2 | Linux fundamentals (01_linux.md Stages 0–5) |
| 3–4 | Networking basics (02_networking.md Stages 0–5) |
| 5–6 | Git mastery (03_git.md all stages) |
| 7–8 | Docker fundamentals (04_docker.md Stages 0–8) |
| 9–10 | Python scripting basics (05_python.md Stages 0–8) |
| 11–12 | Build Portfolio Project #1: Dockerized app + CI/CD |

### Months 4–6: Core Infrastructure
| Week | Focus |
|---|---|
| 13–14 | Terraform basics (08_iac.md Stages 0–4) |
| 15–16 | AWS core services (09_cloud.md Stages 0–5) |
| 17–18 | Kubernetes fundamentals (07_kubernetes.md Stages 0–4) |
| 19–20 | Docker advanced + Compose (04_docker.md Stages 9–15) |
| 21–22 | CI/CD pipelines (11_cicd.md all stages) |
| 23–24 | Build Portfolio Project #2: Terraform + EKS + CI/CD |

### Months 7–9: Observability + Security
| Week | Focus |
|---|---|
| 25–26 | Prometheus + Grafana (10_observability.md Stages 0–5) |
| 27–28 | Log management: Loki/ELK (10_observability.md Stages 6–10) |
| 29–30 | Security basics (12_security.md Stages 0–5) |
| 31–32 | Kubernetes advanced (07_kubernetes.md Stages 5–8) |
| 33–34 | AWS certification prep (SAA) |
| 35–36 | Build Portfolio Project #3: Full monitoring stack |

### Months 10–12: Platform Skills
| Week | Focus |
|---|---|
| 37–38 | Ansible (08_iac.md Ansible stages) |
| 39–40 | Terraform modules + advanced state (08_iac.md Stages 5–8) |
| 41–42 | GitOps with ArgoCD (08_iac.md GitOps stages) |
| 43–44 | Python advanced: OOP, async, testing (05_python.md Stages 13–21) |
| 45–46 | CKA certification prep |
| 47–48 | Build Portfolio Project #4: GitOps platform |

### Months 13–18: Senior-Level Push
| Week | Focus |
|---|---|
| 49–52 | Go fundamentals (06_go.md Stages 0–4) |
| 53–56 | Production engineering (13_production.md) |
| 57–60 | Security advanced: DevSecOps, Vault, OPA (12_security.md) |
| 61–64 | Platform engineering concepts |
| 65–68 | Open source contribution + technical writing |
| 69–72 | Build Portfolio Project #5: Production-grade Platform |

---

## 🔗 Resources for Career Development

### Job Boards (Remote & India)
- https://www.linkedin.com/jobs/ (filter: DevOps, Platform Engineer, SRE)
- https://wellfound.com/ (startups, remote-first)
- https://remote.com/jobs
- https://weworkremotely.com/
- https://devops.com/jobs/
- https://www.naukri.com/ (India)
- https://unstop.com/ (India, newer companies)

### Salary Research
- https://levels.fyi/ (global tech salaries, very accurate)
- https://www.glassdoor.com/Salaries/
- https://www.payscale.com/
- https://survey.stackoverflow.co/2024/ (annual developer survey)

### Interview Prep
- https://github.com/bregman-arie/devops-exercises (600+ DevOps interview questions)
- https://github.com/donnemartin/system-design-primer (system design)
- https://github.com/mxssl/sre-interview-prep-guide (SRE interview prep)
- https://www.techinterviewhandbook.org/ (general interview prep)

### Community
- https://kubernetes.slack.com/ (CNCF Slack — join #general, #k8s-users)
- https://devops.com/ (news, webinars)
- https://community.cncf.io/ (local meetups)
- r/devops, r/sysadmin, r/kubernetes (Reddit)
- Dev.to DevOps tag: https://dev.to/t/devops

---

*Keep this document open. Read it every month and check: which level am I closest to? What's the next skill gap to close?*

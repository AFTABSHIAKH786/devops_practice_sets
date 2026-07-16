# 🏭 Production Engineering — SLOs, Incident Response & Capacity

> **Goal:** Think and operate at the level of a production engineer: define reliability targets, respond to incidents effectively, manage capacity, and reduce toil systematically.
>
> **Target:** Senior DevOps / Platform Engineer level

---

## 📚 Resources

### Primary
- **Google SRE Book (free):** https://sre.google/sre-book/table-of-contents/
- **Google SRE Workbook (free):** https://sre.google/workbook/table-of-contents/
- **The Phoenix Project** — Gene Kim (novel-format, must read)
- **The Unicorn Project** — Gene Kim

### Books
- *Site Reliability Engineering* — Betsy Beyer et al. (Google, free online)
- *Implementing Service Level Objectives* — Alex Hidalgo
- *Accelerate* — Nicole Forsgren et al. (engineering effectiveness data)

### Courses / Videos
- **Google SRE lectures (YouTube):** https://www.youtube.com/watch?v=d2wn_E1jxn4
- **Liz Fong-Jones on SLOs:** https://youtu.be/tEylFyxbDLE

---

## Stage 0 — SRE Fundamentals & the Error Budget

### Learn
- SRE: what is a Site Reliability Engineer? How is it different from DevOps?
- **SLI** (Service Level Indicator): a measurement (e.g., request success rate)
- **SLO** (Service Level Objective): a target (e.g., 99.9% success rate)
- **SLA** (Service Level Agreement): a legal contract (usually looser than SLO)
- **Error Budget:** (1 - SLO) × time window = allowed failure budget
- Toil: manual, repetitive, automatable work — the enemy of SRE
- The SRE vs DevOps overlap: SRE implements DevOps

### Resources
- https://sre.google/sre-book/service-level-objectives/
- https://sre.google/sre-book/eliminating-toil/

### 🟢 Easy Exercises
1. Define an SLI, SLO, and error budget for: a web API, a batch job, and a message queue consumer
2. Calculate: with a 99.9% monthly SLO, how many minutes of downtime are allowed per month? Per week?
3. Make a list of 5 toil tasks in a typical DevOps day — estimate the time each costs weekly

### 🟡 Medium Exercises
1. Write a complete SLO document for a fictional e-commerce API: availability SLI, latency SLI, SLO targets, measurement window, and consequences of burning the error budget
2. Calculate the error budget burn rate for a 30-day window: if you've used 80% of the budget in 10 days, when will you exhaust it?

### 🔴 Hard Exercises
1. Implement multi-window burn rate alerts in Prometheus: alert if burn rate is >14× for 1h OR >6× for 6h (as per Google SRE Workbook)
2. Write a quarterly SLO review document: was the SLO met? Was the error budget consumed? What action does that drive?

---

## Stage 1 — Incident Response

### Learn
- Incident lifecycle: detect → triage → escalate → mitigate → resolve → review
- Severity levels (P0/P1/P2/P3): what each means, response times
- The Incident Commander role
- War room communication: status updates, Slack channels, status pages
- Runbooks: what they are, why they're not optional
- Blameless post-mortems: 5-why analysis, contributing factors, action items
- Status pages: Statuspage.io, Atlassian Status Page, Cachet

### Resources
- https://sre.google/sre-book/managing-incidents/
- https://sre.google/sre-book/postmortem-culture/
- **PagerDuty incident response guide:** https://response.pagerduty.com/

### 🟢 Easy Exercises
1. Write a severity level classification guide for a web platform: what defines P0, P1, P2, P3? Response time for each?
2. Write a runbook for "Database connection pool exhausted" — include: symptoms, investigation steps, mitigation options, and escalation path
3. Read any post-mortem from https://github.com/dastergon/postmortem-collection — identify the 5-why chain and the action items

### 🟡 Medium Exercises
1. Simulate an incident: a Kubernetes pod is OOMKilled repeatedly. Walk through the full incident response process: detect (alert fires), triage, mitigate (increase memory limit), resolve, write post-mortem
2. Write a blameless post-mortem for the simulated incident — use the Google post-mortem template format

### 🔴 Hard Exercises
1. Design a complete incident response framework for a 5-person team: severity levels, roles (IC, comms lead, technical lead), communication templates, escalation paths, on-call rotation
2. Set up PagerDuty or Opsgenie: create schedules, escalation policies, and connect your Prometheus Alertmanager to route alerts appropriately by severity

---

## Stage 2 — Capacity Planning & Cost Engineering

### Learn
- Resource utilization targets: why 100% CPU is bad
- Capacity planning cycle: measure → model → predict → provision
- Horizontal vs vertical scaling
- FinOps: cloud cost visibility, optimization, accountability
- AWS Cost Explorer, Savings Plans, Reserved Instances vs Spot Instances
- Kubernetes resource right-sizing: VPA (Vertical Pod Autoscaler)
- Cluster autoscaling: Cluster Autoscaler, Karpenter

### Resources
- **FinOps Foundation:** https://www.finops.org/
- **AWS Cost Optimization Hub:** https://aws.amazon.com/aws-cost-management/aws-cost-optimization/
- **Karpenter docs:** https://karpenter.sh/

### 🟢 Easy Exercises
1. Pull the last 30 days of AWS Cost Explorer data — identify the top 3 cost drivers for your account
2. List every Kubernetes pod in your cluster without resource requests set — these are capacity planning risks
3. Calculate the monthly cost of running your Kubernetes cluster: EC2 instances + EBS volumes + data transfer

### 🟡 Medium Exercises
1. Run the Kubernetes VPA in recommendation mode for a week — apply its resource recommendations and measure the impact on actual usage
2. Analyze EC2 utilization: identify instances running below 20% CPU for more than 2 weeks — recommend right-sizing

### 🔴 Hard Exercises
1. Set up a FinOps dashboard in Grafana that shows: daily AWS spend by service, cost per namespace in EKS, and month-to-date vs budget
2. Design a Kubernetes cluster autoscaling strategy using Karpenter: different node pools for different workload types (general, memory-optimized, compute-optimized), with Spot instances for non-critical workloads

---

## Stage 3 — Reliability Patterns

### Learn
- Circuit breakers: open, half-open, closed states
- Retries with exponential backoff and jitter
- Timeouts: every network call must have a timeout
- Bulkheads: isolation between components
- Rate limiting: protect downstream services
- Load shedding: gracefully reject excess traffic
- Health checks: liveness, readiness, startup
- Chaos engineering: deliberately inject failure to test resilience

### Resources
- **Netflix Chaos Engineering:** https://netflixtechblog.com/tagged/chaos-engineering
- **Resilience4j:** https://resilience4j.readme.io/
- **Chaos Mesh:** https://chaos-mesh.org/
- *Release It!* — Michael T. Nygard (the reliability patterns book)

### 🟢 Easy Exercises
1. Implement a retry with exponential backoff in Python: max 5 retries, initial delay 1s, multiplier 2×, jitter ±10%
2. Implement a circuit breaker in Python: open after 3 failures, half-open after 10 seconds
3. Explain in writing: why is `retry without backoff` dangerous? What is a "retry storm"?

### 🟡 Medium Exercises
1. Add proper timeouts to all external calls in a sample app: HTTP requests, DB queries, cache operations — document the chosen timeout for each and why
2. Run a chaos experiment with Chaos Mesh: terminate a pod randomly for 5 minutes and observe the impact on your SLO dashboard

### 🔴 Hard Exercises
1. Design the full reliability architecture for a payment service: circuit breakers, timeouts, retries, bulkheads, fallbacks — draw the diagram and justify each decision
2. Run a Game Day: simulate a complete AZ failure in your test cluster and execute your runbook, measuring MTTR

---

## Stage 4 — Platform Engineering Concepts

### Learn
- Internal Developer Platform (IDP): what it is and why it matters
- Developer experience (DevEx): the platform engineer as product manager
- Self-service infrastructure: developers provision what they need without ops tickets
- Backstage: Spotify's open-source IDP
- Golden paths: opinionated, paved roads for common patterns
- Platform team topology (Team Topologies framework)

### Resources
- **Team Topologies book:** https://teamtopologies.com/
- **Backstage docs:** https://backstage.io/docs/
- **Internal Developer Platform:** https://internaldeveloperplatform.org/
- **CNCF Platform Engineering whitepaper:** https://tag-app-delivery.cncf.io/whitepapers/platforms/

### 🟢 Easy Exercises
1. Read the Platform Engineering CNCF whitepaper — summarize the 5 key capabilities of a platform
2. Identify 3 "toil tasks" that developers at a typical company have to do manually that a platform team could automate
3. Install Backstage locally and explore the software catalog, tech radar, and templates

### 🟡 Medium Exercises
1. Design a "golden path" for deploying a new microservice at your company: from repo creation to first production deployment — what is automated, what requires a human?
2. Write a Backstage template that scaffolds a new service: creates GitHub repo, Dockerfile, CI/CD pipeline, and registers the service in the catalog

### 🔴 Hard Exercises
1. Build a simple self-service portal (even a CLI or Slack bot) that lets developers request a new Kubernetes namespace with appropriate RBAC, NetworkPolicy, and ResourceQuota
2. Design the platform team's roadmap for the next quarter: what capabilities will you build? How do you measure success (DORA metrics, developer satisfaction)?

---

## 🏗 Final Capstone — Production Platform

Build a complete production-grade platform from scratch and document every decision:

### Requirements
1. **Infrastructure (Terraform):** VPC, EKS, RDS, S3, IAM — all as code, state in S3 + DynamoDB
2. **Application deployment (Helm + ArgoCD):** GitOps workflow for a 3-tier app
3. **Observability (Prometheus + Grafana + Loki):** metrics, logs, dashboards, SLOs, alerts
4. **Security:** Vault for secrets, Falco for runtime, Kyverno for admission, cosign for images
5. **CI/CD (GitHub Actions):** full pipeline from PR to production
6. **Reliability:** health checks, autoscaling, PDBs, circuit breakers in the app
7. **FinOps:** cost dashboard, resource quotas, VPA recommendations
8. **Documentation:** architecture diagram, runbooks for top 5 alerts, post-mortem template

### Deliverables
- GitHub repository with all code
- `README.md` with architecture diagram and setup instructions
- At least 3 runbooks
- One post-mortem for a real issue you encountered during the build
- `COST_ANALYSIS.md` estimating monthly AWS cost

---

## 🔗 Additional Production Engineering Resources

### Must-Read
- Google SRE Book (free): https://sre.google/sre-book/
- Google SRE Workbook (free): https://sre.google/workbook/
- "How Google Makes Engineers More Productive": https://abseil.io/resources/swe-book/html/toc.html
- Postmortem collection: https://github.com/dastergon/postmortem-collection

### Community
- **SRE Weekly newsletter:** https://sreweekly.com/
- **Increment magazine:** https://increment.com/
- **CNCF blog:** https://www.cncf.io/blog/

---

*Progress: 0/5 stages complete · Est. time to complete: 6–8 weeks at 3 hrs/day*

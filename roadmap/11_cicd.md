# 🔄 CI/CD Pipelines — GitHub Actions, Jenkins & ArgoCD

> **Goal:** Design, build, and maintain production CI/CD pipelines. CI/CD is the heartbeat of every modern software delivery organization.
>
> **Target:** 0–1 year → can build and maintain CI/CD pipelines for real teams

---

## 📚 Resources

### Primary
- **GitHub Actions docs:** https://docs.github.com/en/actions
- **Jenkins docs:** https://www.jenkins.io/doc/
- **ArgoCD docs:** https://argo-cd.readthedocs.io/
- *Continuous Delivery* — Jez Humble & David Farley (the foundational book)

### Courses / Videos
- **TechWorld with Nana — CI/CD Tutorial:** https://youtu.be/R8_veQiYBjI
- **GitHub Actions crash course:** https://youtu.be/R8_veQiYBjI
- **Fireship CI/CD in 100 seconds:** https://youtu.be/scEDHsr3APg
- **KodeKloud GitHub Actions:** https://kodekloud.com/courses/github-actions/

---

## Stage 0 — CI/CD Fundamentals

### Learn
- What is CI? What is CD (Delivery vs Deployment)?
- The software delivery lifecycle
- Pipeline concepts: stages, jobs, steps, artifacts
- Trunk-based development vs long-lived branches
- Deployment strategies: recreate, rolling, blue-green, canary
- The DORA Metrics: deployment frequency, lead time, MTTR, change failure rate

### Resources
- https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment
- **DORA research:** https://dora.dev/research/
- **Google DevOps capabilities:** https://cloud.google.com/architecture/devops

### 🟢 Easy Exercises
1. Draw a complete CI/CD pipeline diagram for a web application: from commit to production. Include every stage and gate.
2. Explain the difference between a rolling deployment and a blue-green deployment — when would you choose each?
3. Calculate DORA metrics for a hypothetical team: 10 deployments/week, 2-hour lead time, 30-min MTTR, 5% failure rate — are they elite, high, medium, or low performers?

---

## Stage 1 — GitHub Actions

### Learn
- Workflow structure: `on`, `jobs`, `steps`
- Events: `push`, `pull_request`, `schedule`, `workflow_dispatch`
- Runners: GitHub-hosted vs self-hosted
- Actions: `actions/checkout`, `actions/setup-python`, `docker/build-push-action`
- Environment variables and secrets: `${{ secrets.NAME }}`
- Job dependencies: `needs`
- Artifacts and caching
- Matrix builds
- Reusable workflows and composite actions

### Resources
- https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions
- **Marketplace actions:** https://github.com/marketplace?type=actions
- **GitHub Actions security hardening:** https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions

### Guided Examples
```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Cache pip dependencies
        uses: actions/cache@v3
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run linting
        run: flake8 .

      - name: Run tests
        run: pytest --cov=. --cov-report=xml

  build-and-push:
    runs-on: ubuntu-latest
    needs: lint-and-test
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push image
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: myapp:${{ github.sha }}
```

### 🟢 Easy Exercises
1. Create a GitHub Actions workflow that runs on every push and prints "Hello from CI"
2. Add a workflow that installs Python dependencies and runs `pytest` on every pull request
3. Add a `workflow_dispatch` trigger so you can manually run the pipeline from the GitHub UI
4. Create a scheduled workflow that runs at midnight UTC every day

### 🟡 Medium Exercises
1. Create a matrix build that tests your app on Python 3.10, 3.11, and 3.12 simultaneously
2. Add artifact upload: after tests, save the coverage report as an artifact downloadable from the Actions UI
3. Add caching for pip dependencies to reduce build time from 60s to under 10s — verify with timing

### 🔴 Hard Exercises
1. Build a complete CI/CD pipeline: lint → test → build Docker image → push to registry → update Kubernetes manifest → create PR with new image tag
2. Set up branch protection + required status checks: PRs cannot be merged until CI passes and a reviewer approves

### 💀 Expert
1. Build a reusable workflow (`workflow_call`) for Docker build+push that's used by 3 different repos
2. Implement a security scan stage using Trivy that fails the pipeline on CRITICAL vulnerabilities and posts results as a PR comment

### 🏗 DevOps Project
Build a production CI/CD pipeline for a real app on GitHub:
- PR: lint, test, security scan, Docker build
- Merge to main: build and push Docker image with git SHA tag
- Auto-update Kubernetes manifest in a separate repo
- ArgoCD picks up the change and deploys to staging
- Manual approval gate before production

---

## Stage 2 — Jenkins

### Learn
- Jenkins architecture: master, agents, plugins
- Freestyle jobs vs Pipeline jobs
- Declarative `Jenkinsfile` syntax
- Shared libraries
- Credentials management in Jenkins
- Jenkins on Kubernetes (Kubernetes plugin)
- Blue Ocean UI

### Resources
- https://www.jenkins.io/doc/book/pipeline/
- **Jenkins official tutorial:** https://www.jenkins.io/doc/tutorials/
- **KodeKloud Jenkins:** https://kodekloud.com/courses/jenkins/

### Guided Examples
```groovy
// Jenkinsfile
pipeline {
    agent any

    environment {
        APP_NAME = 'myapp'
        REGISTRY = 'docker.io/myuser'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'pip install -r requirements.txt && pytest'
            }
        }

        stage('Build Image') {
            steps {
                sh "docker build -t ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER} ."
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "docker login -u $USER -p $PASS && docker push ${REGISTRY}/${APP_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        failure {
            slackSend(message: "Build #${BUILD_NUMBER} failed for ${APP_NAME}")
        }
    }
}
```

### 🟢 Easy Exercises
1. Install Jenkins locally with Docker, create your first Freestyle job, and run it
2. Convert the Freestyle job to a Declarative Pipeline using a `Jenkinsfile` in your repo
3. Add a post-build Slack notification for success and failure

### 🟡 Medium Exercises
1. Set up a Jenkins agent running in a Docker container on demand for each build
2. Create a Jenkins Shared Library with a `buildAndPush(image, tag)` function used across 2 different pipelines
3. Configure Jenkins to require credentials from the credential store — never hardcode secrets in a Jenkinsfile

### 🔴 Hard Exercises
1. Set up Jenkins on Kubernetes using the official Helm chart, with dynamic pod agents
2. Implement a multi-branch pipeline that deploys to dev on feature branches and to prod on main, with different approval gates

---

## Stage 3 — Deployment Strategies in Practice

### Learn
- Rolling deployments on Kubernetes
- Blue-green with two Deployments + Service selector switch
- Canary with Ingress weights (nginx-ingress or Argo Rollouts)
- Feature flags: LaunchDarkly, Flagsmith, Unleash
- Database migrations in CI/CD (forward-only, backward-compatible)

### Resources
- **Argo Rollouts:** https://argoproj.github.io/rollouts/
- **Flagger (progressive delivery):** https://flagger.app/

### 🟢 Easy Exercises
1. Implement a rolling update in Kubernetes: set `maxSurge=1` and `maxUnavailable=0`, trigger the update, watch the pods roll one by one
2. Implement a blue-green deployment: create v2 Deployment, verify it's healthy, then switch the Service selector to v2

### 🟡 Medium Exercises
1. Set up Argo Rollouts for a canary deployment: start at 10%, manual promotion to 50%, then 100%
2. Implement an automated canary with Argo Rollouts: uses Prometheus metrics to auto-promote if error rate < 1%, or auto-rollback if > 5%

---

## Stage 4 — Secrets in CI/CD

### Learn
- Never hardcode secrets in Pipelines or repos
- GitHub Actions secrets: repository and environment secrets
- HashiCorp Vault integration with CI/CD
- AWS Secrets Manager / SSM Parameter Store in pipelines
- OIDC authentication: GitHub Actions → AWS (no long-lived keys)
- Detecting secrets in code: git-secrets, gitleaks, trufflehog

### Resources
- **GitHub OIDC with AWS:** https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services
- **Vault in CI/CD:** https://developer.hashicorp.com/vault/docs/auth/github

### 🟢 Easy Exercises
1. Add a secret to GitHub Actions (e.g., `SLACK_WEBHOOK_URL`) and use it in a workflow without printing it
2. Set up `gitleaks` in a GitHub Actions workflow that scans every PR for accidentally committed secrets

### 🟡 Medium Exercises
1. Configure GitHub Actions to authenticate to AWS using OIDC (Web Identity) — no AWS access keys stored as secrets
2. Pull a secret from AWS Secrets Manager inside a GitHub Actions workflow using OIDC credentials

### 🔴 Hard Exercises
1. Set up HashiCorp Vault with the GitHub auth method so GitHub Actions can fetch Vault secrets without storing a long-lived token
2. Implement pre-commit hooks that scan for secrets before every git commit using `gitleaks`

---

## Stage 5 — Pipeline Testing & Reliability

### Learn
- Testing pipelines: using `act` to run GitHub Actions locally
- Pipeline linting: `actionlint`
- Smoke tests after deployment
- Integration tests in pipelines
- Pipeline observability: build durations, failure rates
- Self-hosted runners: when and why

### Resources
- **act (run GitHub Actions locally):** https://github.com/nektos/act
- **actionlint:** https://github.com/rhysd/actionlint

### 🟢 Easy Exercises
1. Install `act` and run your GitHub Actions workflow locally before pushing to GitHub
2. Run `actionlint` on all your workflow files and fix any warnings

### 🟡 Medium Exercises
1. Add a smoke test stage to your CD pipeline: after deploy, run `curl` against the health endpoint and fail the pipeline if it returns non-200
2. Track pipeline build duration over time in Grafana using GitHub Actions API or Prometheus push gateway

### 🏗 Capstone Project
Build an end-to-end CI/CD platform for a Python web app:
1. GitHub Actions: lint → test → security scan → Docker build → push to ECR
2. On merge to `main`: update image tag in manifests repo, open PR automatically
3. Merge manifests PR → ArgoCD deploys to Kubernetes staging
4. Manual approval in GitHub → ArgoCD deploys to production
5. Post-deploy smoke test via curl health check
6. Slack notification at every stage (success, failure, approval needed)

---

## 🔗 Additional CI/CD Resources

### Tools
- **act (local GitHub Actions):** https://github.com/nektos/act
- **Skaffold (local Kubernetes dev):** https://skaffold.dev/
- **Tilt (local dev + Kubernetes):** https://tilt.dev/
- **Tekton (Kubernetes-native CI/CD):** https://tekton.dev/

### Must-Read
- "DORA State of DevOps Report" https://dora.dev/
- "The Twelve-Factor App — CI/CD principles" https://12factor.net/

---

*Progress: 0/6 stages complete · Est. time to complete: 4–6 weeks at 3 hrs/day*

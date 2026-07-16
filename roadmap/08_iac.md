# 🏗️ Infrastructure as Code — Terraform, Ansible, Helm & GitOps

> **Goal:** Provision, configure, and manage infrastructure declaratively. IaC is the engineering discipline that separates DevOps from sysadmins.
>
> **Target:** 0–1 year → Terraform Associate certified + production IaC practitioner

---

## 📚 Resources

### Terraform
- **Official docs:** https://developer.hashicorp.com/terraform/docs
- **Terraform Registry (modules & providers):** https://registry.terraform.io/
- **Learn Terraform (official tutorials):** https://developer.hashicorp.com/terraform/tutorials
- *Terraform: Up & Running* — Yevgeniy Brikman (best practical book)

### Ansible
- **Official docs:** https://docs.ansible.com/
- **Ansible Galaxy (roles):** https://galaxy.ansible.com/
- *Ansible for DevOps* — Jeff Geerling (free online: https://www.ansiblefordevops.com/)

### Helm
- **Helm docs:** https://helm.sh/docs/
- **ArtifactHub:** https://artifacthub.io/

### GitOps
- **OpenGitOps spec:** https://opengitops.dev/
- **ArgoCD docs:** https://argo-cd.readthedocs.io/
- **Flux docs:** https://fluxcd.io/docs/

### Courses / Videos
- **TechWorld with Nana — Terraform:** https://youtu.be/l5k1ai_GBDE
- **TechWorld with Nana — Ansible:** https://youtu.be/1id6ERvfozo
- **KodeKloud Terraform:** https://kodekloud.com/courses/terraform-for-beginners/
- **freeCodeCamp Terraform:** https://youtu.be/SLB_c_ayRMo

---

## Terraform: Stage 0 — Basics (HCL, Providers, Resources)

### Learn
- HCL syntax: blocks, arguments, expressions
- Providers: `terraform`, `aws`, `google`, `azure`
- Resources and data sources
- `terraform init`, `plan`, `apply`, `destroy`
- `terraform fmt` and `terraform validate`
- State file: what it is, why it matters

### Guided Examples
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-devops-practice-bucket-12345"

  tags = {
    Environment = "dev"
    Team        = "platform"
  }
}
```

### 🟢 Easy Exercises
1. Provision a single S3 bucket — run `init`, `plan`, `apply`, then `destroy`
2. Provision an EC2 instance with a specific AMI, instance type, and tags
3. Use the `random` provider to generate a random suffix for a resource name
4. Tag all your resources and verify them in the AWS console

### 🟡 Medium Exercises
1. Provision a VPC with public/private subnets, Internet Gateway, and route tables
2. Provision a security group allowing SSH from your IP only, and HTTP/HTTPS from anywhere
3. Use `terraform fmt` and `terraform validate` as part of your workflow before every apply

### 🔴 Hard Exercises
1. Provision an RDS MySQL instance with a subnet group, parameter group, and automated backups enabled
2. Provision 3 EC2 instances with `count`, then refactor to `for_each` with a map of instance configs

---

## Terraform: Stage 1 — State, Variables & Outputs

### Learn
- Input variables: `variable`, types, defaults, validation
- Output values: `output`, using them between configs
- `terraform.tfvars` and `.auto.tfvars`
- State: `terraform state list`, `show`, `mv`, `rm`
- Remote backend: S3 + DynamoDB for state locking
- `terraform import`

### 🟢 Easy Exercises
1. Define typed input variables for your EC2 config with defaults — pass values via `terraform.tfvars`
2. Define outputs for EC2 public IP and S3 bucket ARN, view with `terraform output`
3. Run `terraform state list` and `terraform state show` on an existing config

### 🟡 Medium Exercises
1. Configure a remote S3 backend with state locking via DynamoDB — migrate local state to it
2. Use `terraform import` to bring an existing manually-created S3 bucket under Terraform management
3. Use `locals` to compute derived values like a combined name prefix reused across multiple resources

### 🔴 Hard Exercises
1. Simulate state drift: change a resource in the console, run `terraform plan`, reconcile it
2. Write a variable `validation` block enforcing only allowed instance types from an approved list

---

## Terraform: Stage 2 — Modules & Advanced Patterns

### Learn
- Module structure: `main.tf`, `variables.tf`, `outputs.tf`
- Calling modules: source, version
- `for_each` over module blocks
- Terraform Registry modules (terraform-aws-modules)
- Workspaces: dev/staging/prod
- `moved` blocks for safe refactoring
- `dynamic` blocks for repeated nested config

### Resources
- https://developer.hashicorp.com/terraform/language/modules/develop
- **terraform-aws-modules:** https://registry.terraform.io/namespaces/terraform-aws-modules

### 🟢 Easy Exercises
1. Refactor a single `main.tf` into a reusable VPC module with its own variables and outputs
2. Call the module twice with different inputs (dev and prod VPC)
3. Create dev and prod workspaces, deploy the same config with different `terraform.tfvars`

### 🟡 Medium Exercises
1. Use `for_each` over a module block to provision the same EC2 module for 3 different services
2. Design nested modules: a `network` module used inside an `app` module
3. Write a Makefile that runs `terraform plan` on PRs and `terraform apply` on merge (in CI)

### 🔴 Hard Exercises
1. Provision an EKS cluster using `terraform-aws-modules/eks` with managed node groups and IRSA
2. Design a multi-account Terraform layout with proper provider aliasing and `terraform_remote_state` for cross-stack references

### 🏗 DevOps Project
Build a reusable Terraform module library: `network` (VPC), `compute` (EC2 + ASG + ALB), `database` (RDS), `eks` (EKS cluster). Compose them in a root module to stand up a full 3-tier environment from one `terraform apply`.

---

## Ansible: Stage 0 — Basics (Inventory, Playbooks, Ad-Hoc)

### Learn
- Inventory: static hosts file, groups, variables
- Ad-hoc commands: `-m`, `-a`
- Playbooks: `hosts`, `tasks`, `vars`, `become`
- Core modules: `command`, `shell`, `copy`, `file`, `package`, `service`, `template`
- Handlers and `notify`
- `when` conditionals and loops

### Resources
- https://docs.ansible.com/ansible/latest/user_guide/playbooks.html
- **Jeff Geerling's Ansible 101 (YouTube):** https://www.youtube.com/playlist?list=PL2_OBreMn7FqZkvMYt6ATmgC0KAGGJNAN

### Guided Examples
```yaml
---
- name: Install and start nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      package:
        name: nginx
        state: present

    - name: Start and enable nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Copy custom config
      copy:
        src: nginx.conf
        dest: /etc/nginx/nginx.conf
      notify: Restart nginx

  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
```

### 🟢 Easy Exercises
1. Write a static inventory file with 3 hosts grouped under `[webservers]`
2. Run `ansible all -m ping` and `ansible webservers -a "uptime"` against your inventory
3. Write a playbook that installs and starts nginx on all webservers
4. Use the `copy` module to place a file on remote hosts

### 🟡 Medium Exercises
1. Write a playbook using `become: true` to run tasks with sudo
2. Use `vars` to parameterize package names and versions
3. Use `when` conditionals to run tasks only on Ubuntu (check `ansible_os_family`)
4. Use `register` to capture command output and use it in a later task

### 🔴 Hard Exercises
1. Write a playbook that installs and configures MariaDB: creates a database, a user, and grants privileges
2. Run `ansible-lint` on your playbooks and fix every best-practice warning

---

## Ansible: Stage 1 — Roles, Vault & Templates

### Learn
- `ansible-galaxy init` to scaffold a role
- Role structure: `tasks/`, `handlers/`, `templates/`, `vars/`, `defaults/`
- Jinja2 templates: `{{ variable }}`, conditionals, loops in templates
- `ansible-vault`: encrypt, decrypt, edit, rekey
- Dynamic inventory (AWS EC2 plugin)

### 🟢 Easy Exercises
1. Refactor your nginx playbook into an Ansible role
2. Write a Jinja2 template for `nginx.conf` with variable substitution (port, server_name, upstream)
3. Encrypt a variables file with `ansible-vault encrypt` and reference it in a playbook

### 🟡 Medium Exercises
1. Build a reusable `mariadb` role configurable for dev/staging/prod
2. Use `ansible-vault` with a vault password file integrated into a CI pipeline
3. Use loops to create multiple MySQL users from a list variable

### 🔴 Hard Exercises
1. Implement dynamic inventory using the AWS EC2 plugin — hosts discovered automatically by tag
2. Write a playbook that performs a rolling restart across a cluster using `serial: 1`

### 🏗 DevOps Project
Build a complete server configuration management system: Ansible roles for `common` (hardening, users, monitoring agent), `nginx` (web server), and `mariadb` (database). Apply them to a 3-host inventory using a single master playbook.

---

## Helm: See `07_kubernetes.md` Stage 6

Helm is covered in depth in the Kubernetes track since it's a Kubernetes-native tool.

---

## GitOps: Stage 0 — ArgoCD

### Learn
- GitOps principles: Git as single source of truth, reconciliation loop, pull vs push
- ArgoCD: architecture, Application resource, sync waves, hooks
- App of Apps pattern
- Kustomize overlays with ArgoCD

### Resources
- **ArgoCD docs:** https://argo-cd.readthedocs.io/
- **ArgoCD tutorial:** https://youtu.be/MeU5_k9ssrs (TechWorld with Nana)

### 🟢 Easy Exercises
1. Install ArgoCD on a local Kubernetes cluster
2. Create an ArgoCD Application pointing to a Git repo with nginx manifests
3. Make a change in Git and watch ArgoCD detect drift and sync

### 🟡 Medium Exercises
1. Set up auto-sync with self-heal and prune enabled
2. Use Kustomize overlays (base + dev/staging/prod) with ArgoCD

### 🔴 Hard Exercises
1. Build a full CI/CD pipeline: GitHub Actions builds image → updates tag in manifest repo → ArgoCD auto-syncs to cluster
2. Configure ArgoCD RBAC so each team can only manage their own Applications

### 🏗 Capstone Project
Design, implement, and document a full GitOps platform:
- Terraform provisions the EKS cluster and AWS resources
- Ansible configures baseline OS settings on nodes
- Helm charts define all applications
- ArgoCD watches the manifest repo and reconciles continuously
- GitHub Actions builds, tests, and updates manifests on merge to main

---

## 🔗 Additional IaC Resources

### Terraform
- **Terraform best practices:** https://www.terraform-best-practices.com/
- **tflint (linter):** https://github.com/terraform-linters/tflint
- **Infracost (cost estimation):** https://www.infracost.io/
- **Terragrunt (DRY Terraform):** https://terragrunt.gruntwork.io/

### Ansible
- **Ansible Galaxy:** https://galaxy.ansible.com/
- **Molecule (role testing):** https://molecule.readthedocs.io/

### Certification: HashiCorp Terraform Associate
- Exam guide: https://developer.hashicorp.com/certifications/infrastructure-automation
- Free practice: https://developer.hashicorp.com/terraform/tutorials/certification-003

---

*Progress: 0/6 stages complete · Est. time to complete: 8–12 weeks at 3 hrs/day*

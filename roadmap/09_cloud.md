# ☁️ Cloud Platforms — AWS & Azure for Platform Engineers

> **Goal:** Build production-grade cloud infrastructure skills. AWS-first with Azure equivalents noted. Platform Engineers must be comfortable provisioning, operating, and securing cloud resources.
>
> **Target:** 0–1 year → AWS Solutions Architect Associate level

---

## 📚 Resources

### AWS
- **AWS Documentation:** https://docs.aws.amazon.com/
- **AWS Getting Started:** https://aws.amazon.com/getting-started/
- **AWS Free Tier:** https://aws.amazon.com/free/ (use this for practice!)
- **AWS Architecture Center:** https://aws.amazon.com/architecture/

### Books
- *AWS Certified Solutions Architect Study Guide* — Ben Piper & David Clinton
- *AWS Security Best Practices* (AWS whitepaper, free)

### Courses / Videos
- **TechWorld with Nana — AWS Tutorial:** https://youtu.be/k1RI5locZE4
- **freeCodeCamp AWS Certified:** https://youtu.be/ubCNZFQZZVQ
- **Stephane Maarek AWS SAA Udemy:** https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/
- **AWS re:Invent YouTube:** https://www.youtube.com/@AWSEventsChannel

### Practice
- **Cloud Guru practice labs:** https://acloudguru.com/
- **AWS Workshops:** https://workshops.aws/
- **KodeKloud AWS Labs:** https://kodekloud.com/

---

## Stage 0 — Cloud Fundamentals & IAM

### Learn
- Cloud service models: IaaS, PaaS, SaaS
- Shared responsibility model
- AWS Global Infrastructure: Regions, Availability Zones, Edge Locations
- **IAM:** Users, Groups, Roles, Policies (JSON)
- Principle of least privilege
- IAM policy evaluation logic: explicit deny > explicit allow > implicit deny
- MFA, Access Keys vs IAM Roles
- AWS CLI setup and configuration

### Resources
- https://docs.aws.amazon.com/IAM/latest/UserGuide/
- https://aws.amazon.com/iam/faqs/

### 🟢 Easy Exercises
1. Create an IAM User with programmatic access (no console), attach the `ReadOnlyAccess` policy, configure the AWS CLI with its credentials
2. Create an IAM Group called `DevOps`, add policies for EC2, S3, and CloudWatch, add a user to the group
3. Write a custom IAM policy in JSON that allows only `s3:GetObject` on a specific bucket
4. Enable MFA on your root account and IAM user

### 🟡 Medium Exercises
1. Create an IAM Role for an EC2 instance to access S3 — attach it to an instance and verify it can read from S3 without access keys
2. Write a policy that allows an IAM user to manage EC2 instances only in `ap-south-1` (Mumbai) — test it
3. Analyze an IAM policy using the IAM Policy Simulator to verify it allows/denies specific actions

### 🔴 Hard Exercises
1. Set up AWS Organizations with multiple accounts (master + dev + prod) and implement Service Control Policies (SCPs) that prevent any action in the production account outside the approved region
2. Implement attribute-based access control (ABAC) using IAM tags — policy allows access only to resources tagged with the user's team name

### 🏗 DevOps Project
Design and implement the IAM architecture for a small DevOps team: separate roles for Developers (read EC2/S3), DevOps (full EC2/EKS/RDS), and Read-Only (auditors). Document every policy decision.

---

## Stage 1 — Compute: EC2 & Auto Scaling

### Learn
- EC2 instance types (t3, m5, c5, r5 families — know the differences)
- AMIs: public vs private, creating your own
- Key pairs, Security Groups
- EBS volumes: gp3, io2, types and performance
- Elastic IPs
- Auto Scaling Groups: launch templates, scaling policies
- Load Balancers: ALB (Layer 7), NLB (Layer 4)
- EC2 User Data scripts

### Resources
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/
- https://docs.aws.amazon.com/autoscaling/ec2/userguide/

### 🟢 Easy Exercises
1. Launch a `t3.micro` EC2 instance with Amazon Linux 2, connect via SSH, install nginx, and make it publicly accessible
2. Create a Security Group that allows HTTP (80), HTTPS (443), and SSH (22 from your IP only)
3. Create an EBS volume, attach it to an EC2 instance, format it, mount it, write data, detach, attach to another instance, verify data persists
4. Create a custom AMI from a running instance and launch a new instance from it

### 🟡 Medium Exercises
1. Create an Auto Scaling Group with a Launch Template, min=1, max=5, target CPU=50%. Stress the instance and watch it scale
2. Create an ALB, add your ASG as a target group, and verify traffic is distributed across instances
3. Use EC2 User Data to automatically install nginx and deploy a simple HTML page on every new instance

### 🔴 Hard Exercises
1. Implement a blue-green deployment using two ASGs and an ALB: shift traffic from v1 ASG to v2 ASG using listener rules, verify, then switch completely
2. Build a golden AMI pipeline: Packer builds an AMI with your app pre-installed, tested, and tagged with git SHA — triggered by a GitHub Actions workflow

### 🏗 DevOps Project
Deploy a web application to AWS: EC2 behind an ALB, Auto Scaling Group (2–5 instances), health checks, custom metrics alarm in CloudWatch, and a deployment script that triggers a rolling update of the ASG.

---

## Stage 2 — Storage: S3

### Learn
- S3 concepts: buckets, objects, keys, regions
- Storage classes: Standard, Intelligent-Tiering, Standard-IA, Glacier
- Versioning and lifecycle policies
- Bucket policies vs ACLs vs IAM policies
- Pre-signed URLs
- Static website hosting
- S3 event notifications
- Server-side encryption (SSE-S3, SSE-KMS)

### Resources
- https://docs.aws.amazon.com/AmazonS3/latest/userguide/

### 🟢 Easy Exercises
1. Create an S3 bucket, upload a file, set it to public, and access via URL
2. Enable versioning on a bucket, overwrite a file, then retrieve the previous version
3. Set a lifecycle policy to move objects to Glacier after 30 days and delete after 365 days

### 🟡 Medium Exercises
1. Create a bucket policy that allows public read for `/public/*` but requires IAM auth for all other paths
2. Set up S3 event notifications to trigger a Lambda function when a new file is uploaded (or use SQS)
3. Write a Python script using boto3 that uploads all files in a local directory to S3 with a progress bar

### 🔴 Hard Exercises
1. Build a static website hosted on S3, served via CloudFront, with custom domain via Route53 and HTTPS via ACM
2. Implement cross-region replication for a backup bucket — verify replication works and estimate RTO/RPO

### 🏗 DevOps Project
Build an automated S3 backup solution: Bash script that backs up a database dump to S3 with AES-256 encryption, versioning enabled, lifecycle policy for cost management, and a restore script with verification.

---

## Stage 3 — Networking: VPC

### Learn
- VPC: CIDR blocks, subnets (public vs private)
- Internet Gateway, NAT Gateway
- Route Tables
- Security Groups vs NACLs (stateful vs stateless)
- VPC Peering
- VPC Endpoints (S3/DynamoDB)
- Bastion hosts / Session Manager

### Resources
- https://docs.aws.amazon.com/vpc/latest/userguide/

### 🟢 Easy Exercises
1. Create a VPC with CIDR `10.0.0.0/16` with 2 public and 2 private subnets across 2 AZs
2. Create an Internet Gateway, attach to VPC, create route tables
3. Place a web server in a public subnet and a database in a private subnet — verify only the web server is internet-accessible

### 🟡 Medium Exercises
1. Set up a NAT Gateway so private subnet instances can reach the internet for updates but remain unreachable from outside
2. Create a VPC endpoint for S3 so instances in private subnets can access S3 without going through the NAT Gateway

### 🔴 Hard Exercises
1. Design a 3-tier VPC architecture: public (ALB), private-app (EC2/EKS), private-db (RDS) — implement all routing, security groups, and NACLs
2. Set up VPC peering between two VPCs in different accounts and verify cross-VPC connectivity

### 🏗 DevOps Project
Write a Terraform module that creates the standard VPC architecture: 1 VPC, 3 public + 3 private subnets (one per AZ), IGW, NAT Gateway, route tables, and baseline security groups.

---

## Stage 4 — Database Services: RDS & ElastiCache

### Learn
- RDS: managed relational DB (MySQL, PostgreSQL, Aurora)
- Multi-AZ deployments for high availability
- Read replicas for read scaling
- DB parameter groups and option groups
- RDS backup: automated backups, snapshots
- ElastiCache: Redis and Memcached
- DynamoDB basics: tables, items, partition keys, GSI

### Resources
- https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/
- https://docs.aws.amazon.com/AmazonElastiCache/

### 🟢 Easy Exercises
1. Create a `t3.micro` RDS PostgreSQL instance in a private subnet, connect via a bastion host or Session Manager
2. Take a manual snapshot of an RDS instance and restore it to a new instance
3. Create a read replica and verify reads can be directed to it

### 🟡 Medium Exercises
1. Enable Multi-AZ on an RDS instance, simulate a failover, and measure the downtime
2. Set up an ElastiCache Redis cluster, connect to it from an EC2 instance, write and read keys

### 🔴 Hard Exercises
1. Set up cross-region RDS read replica as a DR strategy — document the promotion procedure
2. Implement RDS automated backups with a custom retention window and a Lambda function that verifies backup completion daily

---

## Stage 5 — Container Services: EKS

### Learn
- EKS architecture: managed control plane + node groups
- Managed node groups vs self-managed vs Fargate
- EKS networking: VPC CNI plugin
- IAM Roles for Service Accounts (IRSA)
- EKS add-ons: CoreDNS, kube-proxy, VPC CNI, EBS CSI driver
- eksctl and the AWS console

### Resources
- https://docs.aws.amazon.com/eks/latest/userguide/
- **eksctl:** https://eksctl.io/
- **AWS EKS workshop:** https://www.eksworkshop.com/

### 🟢 Easy Exercises
1. Create an EKS cluster using `eksctl create cluster --name=myCluster --nodes=2`
2. Configure `kubectl` to connect to your EKS cluster
3. Deploy a sample application and expose it via a LoadBalancer Service

### 🟡 Medium Exercises
1. Set up IRSA: create a ServiceAccount linked to an IAM Role so a pod can access S3 without access keys
2. Install the AWS Load Balancer Controller and use it to create an ALB via an Ingress resource

### 🔴 Hard Exercises
1. Provision a production-ready EKS cluster with Terraform: managed node groups, private subnets, IRSA, cluster autoscaler, and the AWS Load Balancer Controller

---

## Stage 6 — Observability: CloudWatch

### Learn
- CloudWatch Metrics: namespaces, dimensions, statistics
- CloudWatch Logs: log groups, log streams, log insights queries
- CloudWatch Alarms: threshold-based, composite alarms
- CloudWatch Dashboards
- AWS X-Ray (distributed tracing)
- AWS Cost Explorer and Budgets

### 🟢 Easy Exercises
1. Create a CloudWatch alarm for EC2 CPU > 80% for 5 minutes that sends an email via SNS
2. Query CloudWatch Logs Insights to find the top 10 error messages in the last 24 hours
3. Create a CloudWatch dashboard showing: EC2 CPU, memory (via agent), ALB request count, and DB connections

### 🟡 Medium Exercises
1. Set up the CloudWatch Agent on an EC2 instance to collect memory and disk metrics (not available by default)
2. Create a composite alarm that only triggers when BOTH CPU AND memory are high simultaneously

### 🏗 DevOps Project
Build a full AWS observability setup for your web app: CloudWatch alarms for all critical metrics, a dashboard, SNS notifications, and Log Insights saved queries for the top 5 operational questions you'd ask in an incident.

---

## Stage 7 — Serverless & Lambda

### Learn
- Lambda: functions, triggers, runtimes, concurrency, cold starts
- Lambda layers
- API Gateway + Lambda as a REST API
- EventBridge for event-driven automation
- SQS / SNS for messaging
- Step Functions for orchestration

### Resources
- https://docs.aws.amazon.com/lambda/latest/dg/
- https://serverlessland.com/

### 🟢 Easy Exercises
1. Write a Lambda function in Python that reads from S3 when a file is uploaded and prints its name
2. Create a Lambda triggered by a CloudWatch Event (now EventBridge) to stop all EC2 instances every evening at 8pm
3. Create a Lambda + API Gateway to expose a "Hello World" REST endpoint

### 🟡 Medium Exercises
1. Build a serverless backup notification system: RDS backup completes → EventBridge → Lambda → Slack webhook
2. Implement a dead-letter queue (DLQ) for a Lambda function to capture failed invocations

---

## 🔗 Additional Cloud Resources

### AWS Cheat Sheets
- https://digitalcloud.training/aws-cheat-sheets/ (excellent for SAA exam prep)
- https://tutorialsdojo.com/aws-cheat-sheets/

### Architecture Reference
- **AWS Well-Architected Framework:** https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html
- **AWS Solutions Library:** https://aws.amazon.com/solutions/

### Cost Management
- **AWS Pricing Calculator:** https://calculator.aws/
- **AWS Cost Anomaly Detection:** https://aws.amazon.com/aws-cost-management/aws-cost-anomaly-detection/

### Azure Equivalents Quick Reference
| AWS | Azure |
|---|---|
| EC2 | Virtual Machines |
| S3 | Blob Storage |
| RDS | Azure SQL / Cosmos DB |
| VPC | Virtual Network (VNet) |
| IAM | Azure Active Directory / RBAC |
| EKS | AKS (Azure Kubernetes Service) |
| Lambda | Azure Functions |
| CloudWatch | Azure Monitor |
| Route53 | Azure DNS |

---

*Progress: 0/8 stages complete · Est. time to complete: 8–12 weeks at 3 hrs/day*

# ☸️ Kubernetes — Objects to Production Operators

> **Goal:** Master Kubernetes from basic pod operations to running production-grade platforms. The most in-demand DevOps skill of the decade.
>
> **Target:** 0–1 year → CKA-ready Kubernetes operator

---

## 📚 Resources

### Primary
- **Official Kubernetes docs:** https://kubernetes.io/docs/home/
- **Kubernetes the Hard Way:** https://github.com/kelseyhightower/kubernetes-the-hard-way
- **kubectl cheat sheet:** https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### Books
- *Kubernetes in Action* — Marko Luksa (best book for deep understanding)
- *Kubernetes: Up and Running* — Kelsey Hightower et al.
- *Production Kubernetes* — Josh Rosso et al.

### Courses / Videos
- **TechWorld with Nana — K8s Tutorial:** https://youtu.be/X48VuDVv0do
- **KodeKloud CKA Course:** https://kodekloud.com/courses/certified-kubernetes-administrator-cka/
- **Mumshad Mannambeth CKA Udemy:** https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/

### Practice Labs
- **KodeKloud K8s labs:** https://kodekloud.com/playgrounds/
- **Killer.sh (CKA practice exams):** https://killer.sh/
- **K8s by example:** https://k8sbyexample.com/
- **Play with Kubernetes:** https://labs.play-with-k8s.com/

### Local Cluster
- **minikube:** https://minikube.sigs.k8s.io/docs/
- **kind (K8s in Docker):** https://kind.sigs.k8s.io/
- **k3s (lightweight):** https://k3s.io/

---

## Stage 0 — Core Concepts & Architecture

### Learn
- What problem does Kubernetes solve?
- Control plane components: API Server, etcd, Scheduler, Controller Manager
- Worker node components: kubelet, kube-proxy, container runtime
- kubectl: the CLI for Kubernetes
- Namespaces: logical isolation
- `kubectl get`, `describe`, `apply`, `delete`, `logs`, `exec`

### Guided Examples
```bash
# Install kubectl and configure kubeconfig
kubectl version --client
kubectl config view

# Explore the cluster
kubectl get nodes
kubectl get namespaces
kubectl get all -n kube-system

# Run your first pod
kubectl run nginx --image=nginx
kubectl get pods
kubectl describe pod nginx
kubectl logs nginx
kubectl exec -it nginx -- bash
kubectl delete pod nginx
```

### 🟢 Easy Exercises
1. Create a Pod running nginx with `kubectl run`, then inspect it with `describe`
2. Write a Pod YAML manifest from scratch and apply it with `kubectl apply -f`
3. Check pod logs and `exec` into the pod to run `curl localhost`
4. Create a pod in a custom namespace called `dev`
5. Delete a pod — what happens if it's managed by a Deployment?

### 🟡 Medium Exercises
1. Create a pod that sets environment variables and a volume from a ConfigMap
2. Find all pods in all namespaces that are NOT in Running state
3. Use `kubectl get events --sort-by=.lastTimestamp` to trace what happened when a pod failed to start

### 🏗 DevOps Project
Deploy a multi-container pod (nginx + a sidecar that writes logs to a shared emptyDir volume) and demonstrate reading the shared logs.

---

## Stage 1 — Deployments & ReplicaSets

### Learn
- ReplicaSets: desired number of pod replicas
- Deployments: manage ReplicaSets for rolling updates
- `kubectl scale`, `kubectl rollout status`, `kubectl rollout undo`
- Rolling update strategy: `maxSurge`, `maxUnavailable`
- Labels and selectors

### 🟢 Easy Exercises
1. Create a Deployment with 3 replicas of nginx, then scale to 5
2. Delete one pod from the Deployment and watch it get recreated
3. Perform a rolling update by changing the image tag
4. Roll back the update with `kubectl rollout undo`
5. List pods with a specific label using `-l app=nginx`

### 🟡 Medium Exercises
1. Write a Deployment YAML with: resource requests/limits, rolling update strategy (maxSurge=1, maxUnavailable=0), and a readiness probe
2. Implement a manual blue-green deployment: two Deployments (v1 and v2) behind one Service — switch traffic by changing the Service selector

### 🔴 Hard Exercises
1. Implement a canary deployment: v2 gets 10% of traffic (1 replica out of 10 total)
2. Debug a Deployment stuck in rollout due to a bad readiness probe — fix it without downtime

---

## Stage 2 — Services & Networking

### Learn
- Service types: ClusterIP, NodePort, LoadBalancer, ExternalName
- Headless services
- Service discovery via DNS: `servicename.namespace.svc.cluster.local`
- `kubectl port-forward`
- Endpoints
- Ingress and Ingress controllers (nginx-ingress)

### Resources
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/concepts/services-networking/ingress/

### 🟢 Easy Exercises
1. Create a ClusterIP Service for your nginx Deployment and curl it from another pod
2. Create a NodePort Service and access it from your host machine
3. Use `kubectl port-forward` to reach the pod directly
4. Inspect Service endpoints with `kubectl get endpoints`

### 🟡 Medium Exercises
1. Write an Ingress routing `/api` to one Service and `/` to another
2. Debug a Service with no endpoints — find and fix the label-selector mismatch
3. Create a headless Service for a StatefulSet and resolve individual pod DNS names

### 🔴 Hard Exercises
1. Write a NetworkPolicy that denies all ingress traffic by default, then allows only frontend → backend on port 8080
2. Set up cert-manager with a ClusterIssuer to auto-provision TLS certs for an Ingress

---

## Stage 3 — Config, Secrets & Storage

### Learn
- ConfigMaps: literal values, files, env vars, volume mounts
- Secrets: base64 encoding (not encryption), types
- PersistentVolumes (PV) and PersistentVolumeClaims (PVC)
- StorageClasses and dynamic provisioning
- StatefulSets: stable network identity, ordered deployment

### 🟢 Easy Exercises
1. Create a ConfigMap from a literal value, mount it as an env var in a Pod
2. Create a Secret, mount it as a volume, verify the file is readable inside the pod
3. Create a PVC, mount it in a Pod, write a file, delete the pod, create a new one, verify file persists

### 🟡 Medium Exercises
1. Deploy PostgreSQL as a StatefulSet with a PVC per replica using `volumeClaimTemplates`
2. Update a ConfigMap and do a rolling restart to pick up the new values
3. Configure dynamic storage provisioning with a StorageClass

### 🔴 Hard Exercises
1. Integrate Sealed Secrets or SOPS to encrypt secrets safely in Git
2. Set up External Secrets Operator to sync a secret from AWS SSM into a Kubernetes Secret

---

## Stage 4 — Scheduling, Probes & Autoscaling

### Learn
- Liveness, Readiness, Startup probes
- Resource requests and limits
- Node affinity and pod affinity/anti-affinity
- Taints and tolerations
- HorizontalPodAutoscaler (HPA)
- CronJobs and Jobs

### 🟢 Easy Exercises
1. Add a liveness probe to a Deployment, break the health endpoint, observe the pod restart
2. Add a readiness probe, verify the pod is removed from Service endpoints on failure
3. Set CPU/memory requests and limits, check `kubectl describe node` allocation

### 🟡 Medium Exercises
1. Set up HPA based on CPU utilization, then load-test to trigger scaling
2. Use node affinity to schedule a workload only on nodes labeled `disktype=ssd`
3. Configure a startup probe for a slow-starting app so liveness doesn't kill it prematurely

### 🔴 Hard Exercises
1. Configure taints on a node and tolerations on a pod so only DB workloads run there
2. Implement Pod Disruption Budgets to protect a StatefulSet during node drains

---

## Stage 5 — Security & RBAC

### Learn
- ServiceAccounts
- Roles and ClusterRoles
- RoleBindings and ClusterRoleBindings
- Pod Security Standards (restricted, baseline, privileged)
- `kubectl auth can-i`
- Network Policies

### Resources
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- https://kubernetes.io/docs/concepts/security/pod-security-standards/

### 🟢 Easy Exercises
1. Create a ServiceAccount, bind it to a Pod, verify it via `exec` and token inspection
2. Create a Role granting get/list on Pods in one namespace, bind with a RoleBinding
3. Use `kubectl auth can-i` to check permissions for a service account

### 🟡 Medium Exercises
1. Create a ClusterRole for read-only access across all namespaces
2. Set a restricted Pod Security Standard at the namespace level and fix a pod that violates it
3. Audit existing RBAC bindings and identify any overly broad cluster-admin grants

### 🔴 Hard Exercises
1. Implement least-privilege RBAC for a CI/CD service account
2. Set up OPA Gatekeeper with a constraint denying Pods that run as root

---

## Stage 6 — Helm & Package Management

### Learn
- Helm charts: templates, values, `Chart.yaml`
- `helm install`, `upgrade`, `rollback`, `uninstall`
- `helm template` for debugging
- Writing your own chart
- Helmfile for managing multiple releases

### Resources
- **Helm docs:** https://helm.sh/docs/
- **ArtifactHub (chart repository):** https://artifacthub.io/
- **Helmfile:** https://github.com/helmfile/helmfile

### 🟢 Easy Exercises
1. Install the bitnami/nginx chart with a custom `values.yaml`
2. Use `helm show values` to inspect chart defaults before installing
3. Override replica count at install time with `--set`

### 🟡 Medium Exercises
1. Write your own Helm chart for a simple app: Deployment + Service + ConfigMap + Ingress with configurable values
2. Use Helm hooks (`pre-install`, `post-upgrade`) to run a DB migration job before the app starts

### 🔴 Hard Exercises
1. Structure a chart with subcharts for a multi-service app
2. Set up a Helmfile to manage multiple releases across dev/staging/prod environments declaratively

---

## Stage 7 — Operators & CRDs

### Learn
- Custom Resource Definitions (CRDs)
- Controllers and the reconciliation loop
- Operator SDK / kubebuilder
- ValidatingWebhookConfiguration and MutatingWebhookConfiguration
- Popular operators: Prometheus Operator, cert-manager, KEDA

### Resources
- **kubebuilder:** https://book.kubebuilder.io/
- **Operator SDK:** https://sdk.operatorframework.io/
- **Operator Hub:** https://operatorhub.io/

### 🟢 Easy Exercises
1. Define a simple CRD (e.g., `Website` with a `url` field) and create an instance
2. Install the Prometheus Operator and create a ServiceMonitor resource
3. Use `kubectl explain` to inspect the schema of a CRD

### 🟡 Medium Exercises
1. Write a basic controller using kubebuilder that watches your CRD and logs changes
2. Create a MutatingWebhook that injects a sidecar container into labeled pods

### 🔴 Hard Exercises
1. Build a controller that reconciles a `BackupSchedule` CRD into a corresponding CronJob
2. Implement a ValidatingWebhook enforcing required annotations on all Deployments

### 🏗 Capstone Project
Build a minimal Kubernetes Operator: define a CRD, implement a controller that reconciles it, deploy with Helm, and write e2e tests with envtest. Document the architecture and publish on GitHub.

---

## 🔗 Additional Kubernetes Resources

### Tools
- **k9s (TUI cluster browser):** https://k9scli.io/
- **Lens (GUI):** https://k8slens.dev/
- **kubectx + kubens:** https://github.com/ahmetb/kubectx
- **stern (multi-pod log tailing):** https://github.com/stern/stern
- **kustomize:** https://kustomize.io/

### Must-Read
- Kubernetes blog: https://kubernetes.io/blog/
- "Kubernetes Networking Guide" https://sookocheff.com/post/kubernetes/understanding-kubernetes-networking-model/
- "RBAC explained" https://www.cncf.io/blog/2018/08/01/demystifying-rbac-in-kubernetes/

### Certification: CKA
- Exam curriculum: https://github.com/cncf/curriculum
- **killer.sh practice exams:** https://killer.sh/ (most realistic CKA prep)

---

*Progress: 0/8 stages complete · Est. time to complete: 8–12 weeks at 3 hrs/day*

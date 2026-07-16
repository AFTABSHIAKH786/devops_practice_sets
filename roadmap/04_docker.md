# 🐳 Docker & Containers

> **Goal:** Go from zero containers to building production-ready Docker images, multi-service Compose setups, and understanding container security.
>
> **Target:** 0–1 year experience → confident Docker practitioner

---

## 📚 Resources

### Primary
- **Official Docker docs:** https://docs.docker.com/
- **Docker Hub:** https://hub.docker.com/
- **Play with Docker (free sandbox):** https://labs.play-with-docker.com/
- **Docker CLI reference:** https://docs.docker.com/engine/reference/commandline/cli/

### Books
- *Docker Deep Dive* — Nigel Poulton (short, practical, great starter)
- *Docker: Up & Running* — Karl Matthias & Sean Kane

### Courses / Videos
- **TechWorld with Nana — Docker Tutorial:** https://youtu.be/3c-iBn73dDE
- **NetworkChuck Docker:** https://youtu.be/eGz9DS-aIeY
- **Docker Mastery (Udemy — Bret Fisher):** https://www.udemy.com/course/docker-mastery/
- **Fireship Docker in 100s:** https://youtu.be/Gjnup-PuquQ

### Practice Labs
- **KodeKloud Docker Labs:** https://kodekloud.com/courses/docker-for-the-absolute-beginner/
- **Play with Docker classroom:** https://training.play-with-docker.com/

---

## Stage 0 — Container Concepts & Docker Basics

### Learn
- What is a container? VMs vs containers
- Docker architecture: daemon, client, registry, images, containers
- Docker image layers — union file system (OverlayFS)
- `docker run`, `docker ps`, `docker stop`, `docker rm`
- Attaching to a container: `-it`, `exec`
- Port mapping: `-p host:container`
- Volumes: `-v host:container`
- Environment variables: `-e`

### Understand
- What is the difference between an image and a container?
- Why are containers more efficient than VMs for microservices?
- What happens when a container restarts? (ephemeral nature)

### Guided Examples
```bash
# Run your first container
docker run hello-world

# Run an interactive Ubuntu container
docker run -it ubuntu bash

# Run nginx in detached mode with port mapping
docker run -d -p 8080:80 --name my-nginx nginx

# See running containers
docker ps

# See all containers (including stopped)
docker ps -a

# Execute a command in running container
docker exec -it my-nginx bash

# View logs
docker logs -f my-nginx

# Stop and remove
docker stop my-nginx && docker rm my-nginx
```

### 🟢 Easy Exercises
1. Run a PostgreSQL container with: name=`mydb`, password=`secret`, port mapped to 5432 — connect to it with `psql` or `docker exec`
2. Run a Redis container, write a key with `redis-cli`, stop the container, start a new one — verify the data is gone
3. Run nginx and serve a custom `index.html` from your local machine using a bind mount
4. List all images on your system, pull `alpine:latest`, and compare its size to `ubuntu:latest`
5. Run a container with a memory limit of 256MB and a CPU limit of 0.5 cores

### 🟡 Medium Exercises
1. Run a container and attach an environment variable file (`--env-file`) instead of individual `-e` flags
2. Explore the layers of the `nginx` image using `docker image inspect` and `docker history`
3. Run a container, make a change inside it, and use `docker diff` to see what changed, then `docker commit` to create a new image (understand why this is bad practice)

### 🔴 Hard Exercises
1. Explain what happens to all data in a container when it is stopped vs when it is removed. Demonstrate with Redis.
2. Run 3 nginx containers and configure them all to share the same volume — explain the risks

### 🏗 DevOps Project
Package a simple web app (any language) as a Docker container: run it locally, verify it works, push the image to Docker Hub.

---

## Stage 1 — Dockerfile & Image Building

### Learn
- `FROM`, `RUN`, `COPY`, `ADD`, `CMD`, `ENTRYPOINT`, `ENV`, `EXPOSE`, `WORKDIR`, `ARG`, `USER`, `LABEL`
- Build context and `.dockerignore`
- Layer caching — order matters
- `CMD` vs `ENTRYPOINT`
- Multi-stage builds — drastically smaller images
- Best practices: one process per container, non-root user, minimal base images

### Resources
- https://docs.docker.com/engine/reference/builder/
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- **Distroless images:** https://github.com/GoogleContainerTools/distroless

### 🟢 Easy Exercises
1. Write a Dockerfile for a "Hello World" Python Flask app — EXPOSE port 5000, use CMD to start it
2. Use `.dockerignore` to exclude `__pycache__`, `.env`, and `*.log` from the build context
3. Build an image with `docker build -t myapp:v1 .` and verify it runs

### 🟡 Medium Exercises
1. Optimize a naive Dockerfile by reordering layers so that `pip install` is cached even when source code changes
2. Rewrite a Dockerfile that runs as root to run as a non-root user `appuser` with UID 1000
3. Write a Dockerfile using ARG for version numbers so you can build `myapp:v1.0` and `myapp:v2.0` from the same file with `--build-arg VERSION=1.0`

### 🔴 Hard Exercises
1. Build a Go application with a multi-stage Dockerfile: stage 1 compiles the binary using `golang:1.22`, stage 2 uses `scratch` to produce a final image under 10MB
2. Build the same Python app with three different base images (`python:3.12`, `python:3.12-slim`, `python:3.12-alpine`) — compare image sizes and explain the trade-offs

### 💀 Expert
1. Write a Dockerfile for a Node.js app that: uses multi-stage build, runs as non-root, uses build cache efficiently, passes a secret (npm token) during build using `--secret` without baking it into the image

### 🏗 DevOps Project
Write production-quality Dockerfiles for three services: a Python API, a Node.js frontend, and a Go utility binary. All must: use non-root users, multi-stage builds, pinned base image versions, and pass a `docker scan` with no critical vulnerabilities.

---

## Stage 2 — Docker Networking

### Learn
- Docker network types: bridge, host, none, overlay, macvlan
- Container-to-container communication on the same network
- DNS resolution between containers by container name
- Exposing ports: publish vs expose
- User-defined networks vs default bridge

### Resources
- https://docs.docker.com/network/
- https://docs.docker.com/network/bridge/

### 🟢 Easy Exercises
1. Create a user-defined bridge network, run two containers on it, and verify they can ping each other by name
2. Run a container with `--network host` and explain the implications
3. Inspect a running container's network config: IP, MAC, gateway

### 🟡 Medium Exercises
1. Set up a 3-container app (nginx → api → redis) where nginx talks to api and api talks to redis, all on a custom network with no port exposure except nginx:80
2. Explain: why can containers on the default bridge network NOT communicate by name, but containers on user-defined networks CAN?

### 🔴 Hard Exercises
1. Configure network isolation: two groups of containers that cannot talk to each other but both can reach the internet — using Docker's `--internal` network flag and iptables rules

---

## Stage 3 — Docker Volumes & Persistent Storage

### Learn
- Volume types: bind mounts, named volumes, tmpfs
- `docker volume create`, `docker volume ls`, `docker volume inspect`
- Volume drivers
- Sharing volumes between containers
- Backup and restore Docker volumes

### 🟢 Easy Exercises
1. Create a named volume, mount it in a container, write data, remove the container, create a new container with the same volume, and verify data persists
2. Bind mount your local source code into a container for live-reload development
3. Inspect a volume and find its location on the host filesystem

### 🟡 Medium Exercises
1. Run a PostgreSQL container with a named volume, insert data, stop+remove the container, recreate it, and verify the data survived
2. Write a backup script that `docker run --volumes-from` to create a tar archive of a container's volume

### 🏗 DevOps Project
Set up a PostgreSQL database container with a named volume and automated daily backup: a script that creates a `pg_dump`, stores it in a timestamped directory, and removes backups older than 7 days.

---

## Stage 4 — Docker Compose

### Learn
- `docker-compose.yml` structure: version, services, networks, volumes
- `image` vs `build`
- `depends_on`, `environment`, `ports`, `volumes`
- `docker compose up`, `down`, `logs`, `exec`, `ps`
- Overrides: `docker-compose.override.yml`
- Health checks in Compose
- Environment files with `.env`

### Resources
- https://docs.docker.com/compose/
- https://docs.docker.com/compose/compose-file/

### 🟢 Easy Exercises
1. Write a `docker-compose.yml` that runs nginx + PostgreSQL with a named volume for the DB
2. Add a health check to the PostgreSQL service so nginx only starts when DB is ready
3. Use a `.env` file to externalize the database password from the Compose file

### 🟡 Medium Exercises
1. Write a Compose file for a 3-tier app: frontend (nginx), backend (Python Flask), database (PostgreSQL) with proper networking (frontend can reach backend, backend can reach DB, frontend cannot reach DB directly)
2. Use `docker-compose.override.yml` to add development-specific settings (volume mounts, debug env vars) without changing the base file

### 🔴 Hard Exercises
1. Set up a full local development environment with Compose: app with live reload, PostgreSQL with persistent data, Redis for caching, pgAdmin as a DB GUI — all starting with `docker compose up -d`
2. Write a Compose file that scales the API service to 3 replicas behind an nginx load balancer

### 💀 Expert
1. Implement health checks for every service in a Compose file, and use `depends_on: condition: service_healthy` to enforce startup ordering strictly

### 🏗 DevOps Project
Containerize a full 3-tier application with Docker Compose: frontend + API + database. Include: `.env` file for secrets, health checks, named volumes for persistence, custom networks for isolation, and a Makefile with `make up`, `make down`, `make logs`, `make backup` targets.

---

## Stage 5 — Docker Security

### Learn
- Running as non-root
- Read-only filesystems: `--read-only`
- Capabilities: drop all, add only needed
- Seccomp profiles
- AppArmor/SELinux
- Image scanning: Trivy, Docker Scout
- Secrets management (not environment variables for production secrets)
- Docker content trust / image signing

### Resources
- https://docs.docker.com/engine/security/
- **Trivy scanner:** https://github.com/aquasecurity/trivy
- **Docker security best practices:** https://snyk.io/learn/docker-security/
- **Container Security book (free chapters):** https://github.com/lizrice/container-security

### 🟢 Easy Exercises
1. Scan the `nginx:latest` image with Trivy — count critical, high, medium vulnerabilities
2. Run a container as a specific user: `--user 1000:1000`
3. Run a container with `--read-only` and mount a tmpfs for `/tmp`

### 🟡 Medium Exercises
1. Compare `nginx:latest` vs `nginx:alpine` vs `nginxinc/nginx-unprivileged` for vulnerabilities using Trivy — which is most secure?
2. Drop all Linux capabilities from a container and add back only what's needed for it to run

### 🔴 Hard Exercises
1. Write a `seccomp` profile for an nginx container that only allows the syscalls nginx actually needs
2. Set up Docker Content Trust: sign an image with your key and verify that Docker refuses to run unsigned images

### 🏗 DevOps Project
Audit all Dockerfiles in your portfolio: run Trivy on all images, fix any critical vulnerabilities, add non-root users, drop unnecessary capabilities, and add a GitHub Actions step that blocks PRs with critical vulnerabilities.

---

## Stage 6 — Production Docker Patterns

### Learn
- Log drivers: json-file, syslog, fluentd, journald
- Resource limits: CPU, memory, ulimits
- Restart policies: no, always, on-failure, unless-stopped
- Docker registry: Docker Hub, ECR, GCR, Harbor (private)
- Image tagging strategy: semantic versioning, git SHA tags
- Rolling updates with Docker
- Container orchestration context (why Kubernetes exists)

### Resources
- https://docs.docker.com/config/containers/logging/
- https://docs.docker.com/config/containers/resource_constraints/
- **Harbor private registry:** https://goharbor.io/

### 🟢 Easy Exercises
1. Run a container with `--restart=unless-stopped` — verify it restarts after stopping but not after `docker stop`
2. Set CPU and memory limits, then use `docker stats` to monitor usage in real time
3. Tag an image with both `latest` and a semantic version `v1.2.3`, push both tags to Docker Hub

### 🟡 Medium Exercises
1. Set up a private container registry using Docker registry image locally — push and pull from it
2. Configure the json-file log driver with log rotation: max size 10MB, max 3 files per container

### 🔴 Hard Exercises
1. Write a rolling update script for a containerized app: pull new image, stop old containers one at a time, start new ones, run health checks, rollback on failure
2. Set up Watchtower to automatically update containers when a new image is pushed to the registry

### 🏗 DevOps Project
Build a production Docker deployment for a web app: ECR registry (or Docker Hub), semantic versioned images, resource limits, log rotation, restart policy, health check, and a deploy script that does zero-downtime rolling updates.

---

## 🔗 Additional Docker Resources

### Cheat Sheets
- https://docs.docker.com/get-started/docker_cheatsheet.pdf
- https://github.com/wsargent/docker-cheat-sheet

### Must-Read
- "Docker and the PID 1 zombie reaping problem" https://blog.phusion.nl/2015/01/20/docker-and-the-pid-1-zombie-reaping-problem/
- "Best practices for writing Dockerfiles" https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

### Tools
- **Dive (explore image layers):** https://github.com/wagoodman/dive
- **Hadolint (Dockerfile linter):** https://github.com/hadolint/hadolint
- **Lazydocker (TUI):** https://github.com/jesseduffield/lazydocker
- **Trivy (scanner):** https://github.com/aquasecurity/trivy

---

*Progress: 0/7 stages complete · Est. time to complete: 4–6 weeks at 3 hrs/day*

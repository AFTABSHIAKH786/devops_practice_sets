# 🌿 Git & Version Control

> **Goal:** Master Git from basics to GitOps workflows. Version control is the foundation of every modern software and infrastructure system.
>
> **Target:** 0–1 year → confident Git user who understands branching strategies and GitOps

---

## 📚 Resources

### Primary
- **Official Git documentation:** https://git-scm.com/doc
- **Pro Git Book (free):** https://git-scm.com/book/en/v2
- **Git Immersion (interactive tutorial):** https://gitimmersion.com/
- **Learn Git Branching (visual, interactive):** https://learngitbranching.js.org/

### Courses / Videos
- **GitHub Git & GitHub Tutorial:** https://docs.github.com/en/get-started
- **Fireship Git in 100 seconds:** https://youtu.be/hwP7WQkmECE
- **TechWorld with Nana — Git Tutorial:** https://youtu.be/RGOj5yH7evk
- **The Git Parable (conceptual):** https://tom.preston-werner.com/2009/05/19/the-git-parable.html

### Practice
- **GitHub Learning Lab:** https://github.com/apps/github-learning-lab
- **Katacoda Git scenarios:** https://www.katacoda.com/ (search Git)
- **Oh Shit, Git!** https://ohshitgit.com/ (fixing common mistakes)

---

## Stage 0 — Git Fundamentals

### Learn
- What is version control and why does it exist?
- Git vs GitHub vs GitLab vs Bitbucket
- `git init`, `git clone`
- The three areas: Working Directory → Staging Area → Repository
- `git add`, `git commit`, `git status`, `git log`
- `.gitignore`
- `git diff` — working directory vs staged vs committed

### 🟢 Easy Exercises
1. Initialize a new Git repo, create 3 files, add them, and commit with a descriptive message
2. Create a `.gitignore` that ignores: `*.log`, `.env`, `node_modules/`, `__pycache__/`
3. Make 5 commits to a repo and use `git log --oneline --graph` to view the history
4. Use `git diff` to see changes before and after staging
5. Use `git show HEAD` to view the last commit's details

### 🟡 Medium Exercises
1. Write a commit message that follows the Conventional Commits spec: https://www.conventionalcommits.org/ for 5 different types of changes
2. Use `git log --since="1 week ago" --author="YourName" --pretty=format:"%h %s"` to see your activity
3. Create a repo with a `.gitignore`, commit some files, then add more ignore patterns and use `git rm --cached` to un-track already committed files

### 🏗 DevOps Project
Initialize a Git repo for your `server_health.sh` script from Linux Stage 5. Add a proper `.gitignore`, commit every version you wrote, tag the stable version as `v1.0.0`.

---

## Stage 1 — Branching & Merging

### Learn
- `git branch`, `git checkout`, `git switch`
- Creating, renaming, and deleting branches
- `git merge`: fast-forward vs 3-way merge
- Merge conflicts: what they are, how to resolve them
- `git rebase`: when to use it vs merge
- `HEAD` — what it points to

### Resources
- https://www.atlassian.com/git/tutorials/using-branches
- https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

### 🟢 Easy Exercises
1. Create a `feature/add-logging` branch, make 3 commits, then merge it back to `main`
2. Create a merge conflict intentionally: edit the same line in two branches, then resolve the conflict
3. Use `git log --all --oneline --graph` to visualize branch history

### 🟡 Medium Exercises
1. Use `git rebase` to rebase a feature branch onto an updated main — explain what happened to the commit history
2. Use `git merge --squash` to squash 5 feature commits into one clean merge commit
3. Create a scenario where a fast-forward merge happens vs where a 3-way merge is required — explain the difference

### 🔴 Hard Exercises
1. Rebase an interactive session: reorder 3 commits, squash 2 together, and edit one commit message
2. Use `git bisect` to find which commit introduced a bug in a dummy repository with 20 commits

---

## Stage 2 — Remote Repositories & Collaboration

### Learn
- `git remote`: add, remove, show
- `git fetch`, `git pull`, `git push`
- Upstream tracking branches
- Pull Requests / Merge Requests
- Forking workflow
- Protected branches and branch policies
- SSH vs HTTPS authentication

### Resources
- https://docs.github.com/en/pull-requests
- https://www.atlassian.com/git/tutorials/syncing

### 🟢 Easy Exercises
1. Fork a public GitHub repo, clone your fork, make a change, push it, and open a Pull Request
2. Add a remote to an existing local repo and push your first branch
3. Use `git fetch origin` (without merge) and explain what it does vs `git pull`

### 🟡 Medium Exercises
1. Set up SSH key authentication with GitHub and push without entering a password
2. Create a repo with protected main branch (no direct push), work on a feature branch, open a PR, and merge it
3. Simulate a team workflow: have two "users" (two terminal windows) push conflicting changes and resolve the conflict via a PR

### 🔴 Hard Exercises
1. Set up a GitHub Actions workflow that runs on every PR and blocks the merge if tests fail
2. Configure branch protection rules: require PR reviews, require CI to pass, no force pushes

---

## Stage 3 — Advanced Git Operations

### Learn
- `git stash`: save, pop, list, apply
- `git cherry-pick`: apply specific commits to another branch
- `git reset`: soft, mixed, hard
- `git revert`: undo without rewriting history
- `git tag`: lightweight vs annotated
- `git reflog`: recovering lost commits
- `git worktree`: multiple working directories

### Resources
- https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging
- **Oh Shit Git:** https://ohshitgit.com/

### 🟢 Easy Exercises
1. Stash your current changes, switch branches, come back, and pop the stash
2. Cherry-pick a specific commit from a `hotfix` branch to `main`
3. Create an annotated tag `v1.0.0` with a message, push it to remote

### 🟡 Medium Exercises
1. Use `git reset --soft HEAD~3` to undo 3 commits but keep the changes staged — then recommit as one
2. Use `git revert` to undo a specific commit in the middle of history without touching other commits
3. Accidentally delete a branch — use `git reflog` to find the commit and recover it

### 🔴 Hard Exercises
1. Using `git filter-repo` or `git filter-branch`, remove a file containing secrets from the entire commit history — explain why this is destructive and what you must do after
2. Set up a Git hook (`pre-commit`) that runs ShellCheck on any `.sh` file being committed and blocks the commit if there are errors

### 🏗 DevOps Project
Set up a Git repo for your infrastructure scripts with: proper branching strategy, conventional commit messages enforced by a pre-commit hook, and a GitHub Actions workflow that validates shell scripts on PR.

---

## Stage 4 — Git Workflows for Teams

### Learn
- **Gitflow:** main, develop, feature/*, release/*, hotfix/*
- **GitHub Flow:** main + feature branches + PR → merge
- **Trunk-Based Development:** commit to main frequently, feature flags
- **GitOps:** Git as the source of truth for infrastructure
- When to use which workflow

### Resources
- https://www.atlassian.com/git/tutorials/comparing-workflows
- **Trunk Based Development:** https://trunkbaseddevelopment.com/
- **GitOps principles:** https://opengitops.dev/

### 🟢 Easy Exercises
1. Draw (or write) the Gitflow branching model — include: when each branch is created, merged, and deleted
2. Implement GitHub Flow for a small project: main → feature → PR → merge cycle × 3
3. Explain: why does Trunk-Based Development work at Google but might be risky for a small team?

### 🟡 Medium Exercises
1. Set up a Gitflow workflow using `git flow` CLI tool or manually — demonstrate the full release cycle
2. Compare: for a 5-person DevOps team maintaining Terraform code, which Git workflow would you recommend and why?

### 🔴 Hard Exercises
1. Implement a complete GitOps workflow: write Terraform code, push to Git, trigger a GitHub Actions pipeline that plans and applies (with manual approval gate for `apply`)
2. Design a branching strategy for a monorepo containing 3 microservices with different release cycles

---

## Stage 5 — Git for Infrastructure (GitOps)

### Learn
- GitOps: reconciliation loop, desired state vs actual state
- ArgoCD: syncs K8s manifests from Git
- Flux: GitOps operator for Kubernetes
- Atlantis: Terraform pull request automation
- Secrets in Git: what NOT to commit, SOPS, git-crypt
- Git as the source of truth for infrastructure

### Resources
- **GitOps specification:** https://opengitops.dev/
- **ArgoCD docs:** https://argo-cd.readthedocs.io/
- **Flux docs:** https://fluxcd.io/docs/
- **Atlantis docs:** https://www.runatlantis.io/docs/
- **SOPS (secrets in Git):** https://github.com/mozilla/sops

### 🟢 Easy Exercises
1. Explain the "pull vs push" model in GitOps — draw a diagram
2. Install ArgoCD on a local Kubernetes cluster (minikube/kind) and connect it to a Git repo
3. Deploy a simple nginx app via ArgoCD — make a change in Git and watch ArgoCD reconcile

### 🟡 Medium Exercises
1. Set up ArgoCD with auto-sync enabled — modify the Git manifest and verify auto-sync triggers without manual intervention
2. Use SOPS to encrypt a secret file in your Git repo and configure ArgoCD to decrypt it at apply time

### 🔴 Hard Exercises
1. Set up Atlantis for a Terraform repo: on PR open → `terraform plan` comment, on PR merge → `terraform apply`
2. Design a GitOps repo structure for a multi-environment Kubernetes platform (dev/staging/prod) with Kustomize overlays

### 🏗 DevOps Project
Set up a GitOps pipeline for your Kubernetes app: manifests in Git → ArgoCD watches → any change in Git is automatically deployed to the cluster within 60 seconds. Document the architecture in a README.

---

## 🔗 Additional Git Resources

### Cheat Sheets
- https://education.github.com/git-cheat-sheet-education.pdf
- https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet

### Must-Read
- "A successful Git branching model" https://nvie.com/posts/a-successful-git-branching-model/
- "Please, commit more often" https://sethrobertson.github.io/GitBestPractices/

### Tools
- **Lazygit (TUI client):** https://github.com/jesseduffield/lazygit
- **tig (terminal Git browser):** https://jonas.github.io/tig/
- **git-delta (better diffs):** https://github.com/dandavison/delta

---

*Progress: 0/6 stages complete · Est. time to complete: 2–3 weeks at 3 hrs/day*

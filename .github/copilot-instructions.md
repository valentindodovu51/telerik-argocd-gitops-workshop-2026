# GitHub Copilot Instructions – Argo CD GitOps Workshop

You are generating files for a **hands-on Argo CD GitOps workshop**.

## 🎯 Context
- Audience: Junior to Mid DevOps engineers
- Environment: Local Kubernetes cluster (minikube / kind / k3d)
- Goal: Teach GitOps, Argo CD reconciliation, drift detection, rollback
- Style: Simple, explicit, production-like but beginner-friendly

This repository is **NOT** a library or application.
It is a **training repository**.

---

## 🧱 Repository Structure (MANDATORY)

Generate files ONLY in the following structure:

.
├── README.md
├── app/
│ ├── namespace.yaml
│ ├── deployment.yaml
│ └── service.yaml
├── argocd/
│ └── application.yaml
├── scenarios/
│ ├── broken-image.md
│ ├── manual-drift.md
│ └── rollback.md
└── cheat-sheet.md


DO NOT:
- invent additional folders
- add Helm, Kustomize, or overlays
- introduce CI pipelines
- introduce cloud-specific resources

---

## ⚙️ Kubernetes Manifests Rules

### General
- Kubernetes API version must be stable (apps/v1, v1)
- All manifests must be **valid YAML**
- Keep manifests minimal and readable
- No advanced features (HPA, Ingress, secrets)

### Namespace
- Namespace name: `demo`

### Deployment
- Name: `demo-app`
- Namespace: `demo`
- Image: `nginx:1.25`
- Replicas: `2`
- Single container
- Expose port `80`
- No resource limits

### Service
- Name: `demo-service`
- Namespace: `demo`
- Type: ClusterIP
- Port: 80 → 80

---

## 🚀 Argo CD Application Rules

- API version: argoproj.io/v1alpha1
- Kind: Application
- Namespace: argocd
- Application name: demo-app
- Repo URL: https://github.com/kdvalkov/telerik-argocd-gitops-workshop-2026
- Path: app
- Target revision: HEAD
- Destination namespace: demo
- Enable:
- automated sync
- prune
- selfHeal

DO NOT:
- add sync waves
- add hooks
- add multiple environments

---

## 📘 Documentation Rules

### README.md
Must include:
- Workshop goal
- Prerequisites
- How to install Argo CD
- How to access Argo CD UI
- High-level GitOps explanation
- Clear warning:
> Do NOT use kubectl apply for application resources

### scenarios/*.md
Each scenario file must include:
- Scenario name
- Goal
- Steps
- Expected Argo CD behavior
- Learning outcome

Scenarios:
- broken-image.md → invalid container image
- manual-drift.md → kubectl scale drift
- rollback.md → git revert rollback

### cheat-sheet.md
Must include:
- Common kubectl commands
- Common Argo CD states
- Troubleshooting tips
- GitOps golden rules

---

## 🧠 Teaching Philosophy (IMPORTANT)

Assume:
- Students will make mistakes
- Things WILL break
- The repo must help them learn from failure

Prefer:
- clarity over cleverness
- repetition over abstraction
- explicit steps over shortcuts

Avoid:
- buzzwords
- unnecessary explanations
- production-hardening concepts

---

## ✅ Output Expectations

When generating files:
- Output ONE FILE AT A TIME
- Respect the repository structure
- Do not summarize
- Do not explain unless explicitly asked

If something is ambiguous:
- choose the simplest possible option


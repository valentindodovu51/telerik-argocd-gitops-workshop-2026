# Argo CD GitOps Workshop

## 🎯 Workshop Goal

This hands-on workshop teaches GitOps principles using Argo CD. You will learn how to:
- Deploy applications using GitOps
- Understand Argo CD reconciliation loops
- Detect and resolve configuration drift
- Rollback changes using Git

## 📋 Prerequisites

Before starting this workshop, you need:

- A local Kubernetes cluster (minikube, kind, or k3d)
- kubectl installed and configured
- Git installed
- Basic knowledge of Kubernetes manifests

## 🚀 How to Install Argo CD

Install Argo CD in your local Kubernetes cluster:

```bash
# Create the argocd namespace
kubectl create namespace argocd

# Install Argo CD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Wait for all pods to be ready
kubectl wait --for=condition=Ready pods --all -n argocd --timeout=300s
```

## 🌐 How to Access Argo CD UI

Get the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Port forward the Argo CD server:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Access the UI at: https://localhost:8080
- Username: `admin`
- Password: (from the command above)

## 📖 What is GitOps?

GitOps is a way of managing application deployments where:

1. **Git is the single source of truth** – All configuration is stored in Git
2. **Declarative configuration** – You define the desired state, not the steps
3. **Automated synchronization** – Tools like Argo CD ensure the cluster matches Git
4. **Version control** – Every change is tracked, auditable, and reversible

### How Argo CD Works

1. You commit Kubernetes manifests to a Git repository
2. Argo CD monitors the Git repository for changes
3. When changes are detected, Argo CD applies them to the cluster
4. Argo CD continuously checks that the cluster state matches Git
5. If someone makes manual changes (drift), Argo CD can detect and fix them

## ⚠️ Important Warning

> **Do NOT use kubectl apply for application resources**
> 
> When using GitOps, all changes must go through Git. Using `kubectl apply` directly creates drift and defeats the purpose of GitOps. Always commit changes to Git and let Argo CD sync them.

## 📂 Repository Structure

```
.
├── README.md
├── app/                    # Application Kubernetes manifests
│   ├── namespace.yaml
│   ├── deployment.yaml
│   └── service.yaml
├── argocd/                 # Argo CD Application definition
│   └── application.yaml
├── scenarios/              # Hands-on learning scenarios
│   ├── broken-image.md
│   ├── manual-drift.md
│   └── rollback.md
└── cheat-sheet.md         # Quick reference guide
```

## 🧪 Workshop Flow

1. Install Argo CD (instructions above)
2. Deploy the demo application: `kubectl apply -f argocd/application.yaml`
3. Work through the scenarios in the `scenarios/` folder
4. Use the cheat sheet as a reference

## 🎓 Learning Outcomes

By the end of this workshop, you will:
- Understand GitOps principles
- Know how to deploy applications with Argo CD
- Be able to detect and fix configuration drift
- Know how to rollback using Git

---

**Ready to start?** Deploy the application and work through the scenarios!
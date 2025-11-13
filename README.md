<div align="center">
  
# GitOps-ArgoCD


![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=for-the-badge&logo=helm&logoColor=white)
![GitOps](https://img.shields.io/badge/GitOps-brightgreen?style=for-the-badge)

**Automated Continuous Delivery with GitOps Principles**

</div>

---

## 🎯 Project Overview

A **GitOps demonstration project** showcasing automated deployment and continuous delivery using ArgoCD and Helm. Git serves as the single source of truth, with ArgoCD automatically syncing application state from repository to Kubernetes cluster.

---

## 🚀 Technology Stack

| 🔧 Tool | 🎯 Purpose | 💎 GitOps Value |
|---|---|---|
| **Kubernetes** | Container Orchestration | Declarative application deployment |
| **Helm** | Package Manager | Templated, reusable manifests |
| **ArgoCD** | GitOps Controller | Automated sync from Git to cluster |
| **Nginx** | Web Server | Sample application deployment |
| **Git** | Version Control | Single source of truth for infrastructure |

---

## 🏗️ Architecture Flow

```
┌─────────────────────────────────────────────────────────┐
│                   GIT REPOSITORY                        │
│         Application Manifests + Helm Charts             │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│                    ARGOCD                               │
├─────────────────────────────────────────────────────────┤
│  1️⃣  Monitors Git Repository                            │
│  2️⃣  Detects Configuration Changes                      │
│  3️⃣  Renders Helm Charts                                │
│  4️⃣  Syncs to Kubernetes Cluster                        │
│  5️⃣  Health Monitoring & Auto-Healing                   │
└────────────────────┬────────────────────────────────────┘
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
    ┌─────────┐ ┌─────────┐ ┌─────────┐
    │Deployment│ │ Service │ │ Ingress │
    │  Pods   │ │Load Bal │ │  Rules  │
    └─────────┘ └─────────┘ └─────────┘
```

---

## 🚀 Quick Start

### 📋 Prerequisites

```bash
# Verify installations
minikube version      # Local Kubernetes cluster
kubectl version       # Kubernetes CLI
helm version          # Helm package manager
```

### ⚡ Setup & Execution

```bash
# 1️⃣ Start Minikube
minikube start --driver=docker

# 2️⃣ Install ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Wait for ArgoCD pods to be ready
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=argocd-server -n argocd --timeout=300s

# 3️⃣ Access ArgoCD UI
kubectl port-forward svc/argocd-server -n argocd 8080:443

# 4️⃣ Get admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# 5️⃣ Login via CLI (optional)
argocd login localhost:8080 --username admin --password <password-from-above>

# 6️⃣ Create ArgoCD application
kubectl apply -f argocd-app.yaml
# OR via CLI:
argocd app create my-app \
  --repo https://github.com/yourusername/gitops-demo \
  --path helm-charts/nginx \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default

# 7️⃣ Sync application
argocd app sync my-app

# 8️⃣ Access deployed application
kubectl port-forward svc/nginx-service 3000:80
```

---

## 📁 Repository Structure

```
gitops-argocd-demo/
├────────────Chart.yaml              # Helm chart metadata
│       ├── values.yaml             # Default configuration
│       └── templates/
│           ├── deployment.yaml     # K8s deployment manifest
│           ├── service.yaml        # K8s service manifest
└── README.md                       # This file
```

---

## 🔄 GitOps Workflow

| Stage | What Happens | GitOps Principle |
|---|---|---|
| **Git Commit** | Developer pushes manifest changes | Git as single source of truth |
| **ArgoCD Detection** | Polls repository for changes | Automated change detection |
| **Helm Rendering** | Templates rendered with values | Declarative configuration |
| **Auto-Sync** | Applies changes to cluster | Continuous reconciliation |
| **Health Check** | Monitors pod/service status | Self-healing infrastructure |

---

## ⚔️ Challenges & Solutions

| Challenge | Root Cause | Solution |
|---|---|---|
| **ArgoCD UI Not Loading** | Port forwarding timeout | Increase timeout, check pod readiness |
| **Application Out of Sync** | Manual kubectl changes | Enable auto-sync in ArgoCD |
| **Helm Chart Errors** | Invalid template syntax | Use `helm template` to debug locally |
| **Image Pull Issues** | Private registry auth | Store credentials in K8s secrets |
| **Sync Failed** | Git repo not accessible | Verify SSH keys or HTTPS credentials |

---

## 📊 Validation Checklist

✅ **Minikube running**: `minikube status`  
✅ **ArgoCD installed**: `kubectl get pods -n argocd`  
✅ **ArgoCD UI accessible**: `https://localhost:8080`  
✅ **Application synced**: Check ArgoCD dashboard  
✅ **Pods healthy**: `kubectl get pods`  
✅ **Service reachable**: `curl http://localhost:3000`  

---

## 🎯 Production Readiness Gap

| Current State | Production Requirement |
|---|---|---|
| Minikube (single-node) | Multi-node managed cluster (EKS/GKE/AKS) |
| HTTP Git access | SSH keys with limited permissions |
| Admin credentials in secrets | OIDC/SSO authentication |
| Manual port-forwarding | Ingress controller with TLS |
| Single environment | Multiple environments (dev/staging/prod) |

---

## 👨‍💻 About This Project

**⚡ Built to Demonstrate GitOps Principles**

This project showcases:
- **Declarative Infrastructure**: Everything defined in Git
- **Automated Sync**: No manual kubectl commands needed
- **Self-Healing**: ArgoCD corrects configuration drift
- **Audit Trail**: Full deployment history in Git commits
- **Rollback Simplicity**: Git revert = instant rollback

> "With GitOps, your Git repository becomes your deployment dashboard."

---

<div align="center">

⭐ **Star this repo if it helps you learn GitOps!** ⭐

Built with Kubernetes | ArgoCD | Helm | GitOps-First Mindset

</div>


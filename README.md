# Kubernetes GitOps DevOps Project

## Project Overview

This project demonstrates a complete end-to-end DevOps implementation using:

- Docker
- Kubernetes
- Helm
- GitHub Actions
- ArgoCD
- Prometheus
- Grafana

The application is containerized using Docker, deployed on Kubernetes using Helm charts, monitored using Prometheus & Grafana, and fully automated using CI/CD + GitOps methodology.

---

# Technologies Used

- Docker
- Kubernetes
- Helm
- GitHub Actions
- ArgoCD
- GitOps
- DockerHub
- NGINX Ingress
- HPA (Horizontal Pod Autoscaler)
- Prometheus
- Grafana

---

# High Level Architecture Diagram

```
                    +------------------+
                    |    Developer     |
                    +------------------+
                              |
                              v
                    +------------------+
                    |      GitHub      |
                    | Source Code Repo |
                    +------------------+
                              |
                              v
                 +------------------------+
                 | GitHub Actions CI/CD   |
                 +------------------------+
                 | Build Docker Image     |
                 | Push Image to DockerHub|
                 | Update Helm values.yaml|
                 | Push Changes to GitHub |
                 +------------------------+
                              |
                              v
                    +------------------+
                    |    DockerHub     |
                    +------------------+
                              |
                              v
                    +------------------+
                    |      ArgoCD      |
                    |   GitOps CD      |
                    +------------------+
                    | Auto Sync        |
                    | Self Heal        |
                    | Detect Git Change|
                    +------------------+
                              |
                              v
              +--------------------------------+
              |      Kubernetes Cluster        |
              +--------------------------------+
              | Deployment                     |
              | Service                        |
              | ConfigMap                      |
              | Secret                         |
              | HPA                            |
              | Ingress                        |
              +--------------------------------+
                              |
                              v
                    +------------------+
                    |   Application    |
                    |      Pods        |
                    +------------------+
                              |
                              v
                    +------------------+
                    |  NGINX Ingress   |
                    +------------------+
                              |
                              v
                    +------------------+
                    | local.amazon.com |
                    +------------------+


               +------------------------+
               |   Monitoring Stack     |
               +------------------------+
               | Prometheus             |
               | Grafana                |
               +------------------------+
```

---

# Features Implemented

## Docker

- Created custom Dockerfile
- Containerized application
- Docker image optimization

---

## Kubernetes

- Deployment
- Service
- ConfigMap
- Secret
- Namespace
- Volume Mounts

---

## Helm

- Created reusable Helm chart
- Dynamic image repository and tag handling using values.yaml
- Parameterized deployment templates

Example:

```
image:
  repository: 9561494547/amazon
  tag: v10
```

Deployment Template:

```
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

---

# CI Pipeline using GitHub Actions

Pipeline Steps:

1. Checkout source code
2. Login to DockerHub
3. Build Docker image
4. Push image to DockerHub
5. Update Helm values.yaml
6. Commit updated values.yaml
7. Push changes to GitHub

---

# Dynamic Docker Image Tagging

Implemented automatic image tagging using GitHub run number.

Example:

```
- name: Set Image Tag
  run: echo "IMAGE_TAG=v${{ github.run_number }}" >> $GITHUB_ENV
```

Generated Tags:

```
v1
v2
v3
v10
```

---

# Automatic Helm values.yaml Update

```
- name: Update Helm Chart Image Tag
  run: |
    sed -i "s/tag:.*/tag: ${IMAGE_TAG}/g" amazon-chart/values.yaml
```

---

# Automatic Git Commit from Pipeline

```
- name: Commit Updated values.yaml
  run: |
    git config --global user.name "github-actions"
    git config --global user.email "github-actions@github.com"

    git add amazon-chart/values.yaml
    git commit -m "Updated image tag to ${IMAGE_TAG}" || echo "No changes to commit"

    git push
```

---

# ArgoCD GitOps Implementation

Implemented ArgoCD for Continuous Deployment.

Features:
- Auto Sync
- Self Healing
- GitOps Deployment
- Automatic Kubernetes Updates

Workflow:
1. GitHub Actions updates values.yaml
2. Changes pushed to GitHub
3. ArgoCD detects Git changes
4. ArgoCD automatically syncs Kubernetes deployment

---

# Horizontal Pod Autoscaler (HPA)

Implemented HPA for automatic scaling based on CPU utilization.

Features:
- Auto scaling
- Resource requests and limits
- Improved availability

---

# Ingress

Implemented NGINX Ingress Controller for external access.

Application accessible using:

```
local.amazon.com
```

---

# Monitoring using Prometheus & Grafana

## Prometheus

- Kubernetes metrics collection
- Pod monitoring
- Node monitoring
- CPU & Memory metrics

## Grafana

- Real-time dashboards
- Kubernetes visualization
- Cluster health monitoring

---

# Useful Commands

## Check Pods

```
kubectl get pods -n oggy
```

## Check Deployment

```
kubectl get deploy -n oggy
```

## Describe Deployment

```
kubectl describe deployment oggy-web -n oggy
```

## Verify Docker Image Version

```
kubectl describe pod <pod-name> -n oggy
```

Example Output:

```
Pulling image "9561494547/amazon:v10"
```

---

# ArgoCD Installation

```
helm repo add argo https://argoproj.github.io/argo-helm

helm repo update

helm install argocd argo/argo-cd --namespace argocd --create-namespace
```

---

# Access ArgoCD UI

```
kubectl port-forward service/argocd-server -n argocd 8080:443
```

Open Browser:

```
https://localhost:8080
```

---

# Get ArgoCD Admin Password

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

---

# Project Outcome

Successfully implemented:

- CI/CD Pipeline using GitHub Actions
- GitOps Continuous Deployment using ArgoCD
- Kubernetes Deployment Automation
- Dynamic Docker Image Tagging
- Automatic Helm Chart Updates
- Kubernetes Monitoring using Prometheus & Grafana
- Auto Scaling using HPA
- Self-Healing Deployment Strategy

---

# Author

Akshay Marathe

GitHub Repository:
https://github.com/aksahram/kubernetes-project
``

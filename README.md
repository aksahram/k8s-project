# Kubernetes GitOps DevOps Project

## Project Overview

This project demonstrates a complete end-to-end DevOps implementation using Docker, Kubernetes, Helm, GitHub Actions, ArgoCD, Prometheus, and Grafana with GitOps methodology.

The application is containerized using Docker, deployed on Kubernetes using Helm charts, monitored using Prometheus and Grafana, and fully automated using CI/CD pipelines.

---

# Technologies Used

- Docker
- Kubernetes
- Helm
- GitHub Actions
- ArgoCD
- GitOps
- DockerHub
- Horizontal Pod Autoscaler (HPA)
- NGINX Ingress
- Prometheus
- Grafana

---

# High Level Architecture Diagram

.
                                      ┌──────────────────────────┐
                                      │        Developer          │
                                      │  Pushes Code to GitHub   │
                                      └────────────┬─────────────┘
                                                   │
                                                   ▼
                           ┌─────────────────────────────────────────┐
                           │             GitHub Repository           │
                           │-----------------------------------------│
                           │ Source Code                             │
                           │ Helm Charts                             │
                           │ Kubernetes Manifests                    │
                           │ values.yaml                             │
                           └────────────────┬────────────────────────┘
                                            │
                                            ▼
                      ┌────────────────────────────────────────────────────┐
                      │               GitHub Actions CI                    │
                      │----------------------------------------------------│
                      │ 1. Checkout Source Code                            │
                      │ 2. Login to DockerHub                              │
                      │ 3. Build Docker Image                              │
                      │ 4. Push Docker Image to DockerHub                  │
                      │ 5. Update Helm values.yaml with New Image Tag      │
                      │ 6. Commit Updated values.yaml                      │
                      │ 7. Push Changes Back to GitHub                     │
                      └──────────────────────┬─────────────────────────────┘
                                             │
                                             ▼
                          ┌────────────────────────────────────┐
                          │            DockerHub               │
                          │------------------------------------│
                          │  Amazon Application Images         │
                          │                                    │
                          │  amazon:v1                         │
                          │  amazon:v2                         │
                          │  amazon:v10                        │
                          └────────────────┬───────────────────┘
                                           │
                                           ▼
                      ┌────────────────────────────────────────────────────┐
                      │                    ArgoCD                         │
                      │---------------------------------------------------│
                      │  GitOps Continuous Deployment                     │
                      │                                                   │
                      │  • Watches GitHub Repository                      │
                      │  • Detects values.yaml Changes                    │
                      │  • Automatically Syncs Kubernetes                 │
                      │  • Self Healing                                   │
                      │  • Auto Sync                                      │
                      │  • Prune Resources                                │
                      └──────────────────────┬────────────────────────────┘
                                             │
                                             ▼
             ┌───────────────────────────────────────────────────────────────────┐
             │                      Kubernetes Cluster                           │
             │------------------------------------------------------------------│
             │ Namespace: oggy                                                  │
             │                                                                  │
             │  ┌────────────────────────────────────────────────────────────┐  │
             │  │                    Kubernetes Resources                    │  │
             │  │------------------------------------------------------------│  │
             │  │ Deployment                                                 │  │
             │  │ Service                                                    │  │
             │  │ ConfigMap                                                  │  │
             │  │ Secret                                                     │  │
             │  │ HPA (Horizontal Pod Autoscaler)                            │  │
             │  │ Ingress                                                    │  │
             │  └──────────────────────┬─────────────────────────────────────┘  │
             │                         │                                        │
             │                         ▼                                        │
             │          ┌────────────────────────────────────┐                  │
             │          │           ReplicaSet               │                  │
             │          └────────────────┬───────────────────┘                  │
             │                           │                                      │
             │                           ▼                                      │
             │          ┌────────────────────────────────────┐                  │
             │          │            Running Pods            │                  │
             │          │------------------------------------│                  │
             │          │  Amazon Application Pods           │                  │
             │          │  Docker Image: amazon:v10          │                  │
             │          └────────────────┬───────────────────┘                  │
             └───────────────────────────┼──────────────────────────────────────┘
                                         │
                                         ▼
                    ┌──────────────────────────────────────────┐
                    │         NGINX Ingress Controller         │
                    │------------------------------------------│
                    │  Host Routing                            │
                    │  local.amazon.com                        │
                    └────────────────┬─────────────────────────┘
                                     │
                                     ▼
                          ┌───────────────────────┐
                          │       End Users       │
                          │ Access Application    │
                          └───────────────────────┘


             ┌────────────────────────────────────────────────┐
             │               Monitoring Stack                 │
             │------------------------------------------------│
             │                                                │
             │  Prometheus                                    │
             │  • Collects Kubernetes Metrics                 │
             │  • Monitors Pods & Nodes                       │
             │  • Tracks CPU & Memory Usage                   │
             │                                                │
             │  Grafana                                       │
             │  • Real-Time Dashboards                        │
             │  • Visualization & Monitoring                  │
             │  • Cluster Health Monitoring                   │
             │                                                │
             └────────────────────────────────────────────────┘
```

---

# Features Implemented

## Docker

- Created custom Dockerfile
- Containerized application
- Optimized image build

---

## Kubernetes

- Deployment
- Service
- ConfigMap
- Secret
- Namespace
- Volume Mounts

---

# Helm

- Created reusable Helm chart
- Dynamic image repository and tag handling using values.yaml
- Parameterized deployment templates

Example:

```yaml
image:
  repository: 9561494547/amazon
  tag: v10
```

Deployment template:

```yaml
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

---

# Horizontal Pod Autoscaler (HPA)

Implemented HPA for automatic scaling based on CPU utilization.

Features:
- Auto scaling
- Resource requests and limits
- Improved application availability

---

# Ingress

Implemented NGINX Ingress Controller for external access.

Application accessible using:

```bash
local.amazon.com
```

---

# CI Pipeline using GitHub Actions

Implemented automated CI pipeline.

Pipeline Steps:
1. Checkout code
2. Login to DockerHub
3. Build Docker image
4. Push image to DockerHub
5. Update Helm values.yaml automatically
6. Commit updated values.yaml
7. Push changes back to GitHub

---

# Dynamic Docker Image Tagging

Implemented automatic image tagging using GitHub run number.

Example:

```yaml
- name: Set Image Tag
  run: echo "IMAGE_TAG=v${{ github.run_number }}" >> $GITHUB_ENV
```

This generates tags like:

```bash
v1
v2
v3
v10
```

---

# Build and Push Docker Image

```yaml
- name: Build Docker Image
  run: |
    docker build -t 9561494547/amazon:${IMAGE_TAG} -f docker/Dockerfile .

- name: Push Docker Image
  run: |
    docker push 9561494547/amazon:${IMAGE_TAG}
```

---

# Automatic Helm values.yaml Update

```yaml
- name: Update Helm Chart Image Tag
  run: |
    sed -i "s/tag:.*/tag: ${IMAGE_TAG}/g" amazon-chart/values.yaml
```

---

# Automatic Git Commit from Pipeline

```yaml
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

Implemented ArgoCD for Continuous Deployment using GitOps.

Features:
- Automatic sync
- Self healing
- Auto deployment
- Git as single source of truth

---

# ArgoCD Sync Policy

Enabled:
- Auto Sync
- Prune Resources
- Self Heal

---

# GitOps Workflow

1. Developer pushes code
2. GitHub Actions pipeline starts
3. Docker image builds automatically
4. Image pushed to DockerHub
5. Helm values.yaml updated automatically
6. Changes pushed to GitHub
7. ArgoCD detects Git changes
8. Kubernetes deployment updated automatically

---

# Monitoring Implementation

Implemented Kubernetes monitoring using Prometheus and Grafana.

## Prometheus

Features:
- Kubernetes metrics collection
- Pod monitoring
- Node monitoring
- Resource usage monitoring

Installed using Helm.

Example:

```bash
helm install prometheus prometheus-community/prometheus
```

---

## Grafana

Features:
- Real-time dashboards
- CPU monitoring
- Memory monitoring
- Pod visualization
- Cluster monitoring

Installed using Helm.

Example:

```bash
helm install grafana grafana/grafana
```

---

# Monitoring Stack Benefits

- Real-time monitoring
- Better observability
- Performance tracking
- Resource utilization analysis
- Kubernetes cluster health monitoring

---

# Monitoring Workflow

Kubernetes Cluster → Prometheus Collects Metrics → Grafana Visualizes Dashboards

---

# Verification Commands

## Check Pods

```bash
kubectl get pods -n oggy
```

---

## Check Deployment

```bash
kubectl get deploy -n oggy
```

---

## Describe Deployment

```bash
kubectl describe deployment oggy-web -n oggy
```

---

## Verify Image Version

```bash
kubectl describe pod <pod-name> -n oggy
```

Example Output:

```bash
Pulling image "9561494547/amazon:v10"
```

---

# ArgoCD Installation

```bash
helm repo add argo https://argoproj.github.io/argo-helm

helm repo update

helm install argocd argo/argo-cd --namespace argocd --create-namespace
```

---

# Access ArgoCD UI

```bash
kubectl port-forward service/argocd-server -n argocd 8080:443
```

Open Browser:

```bash
https://localhost:8080
```

---

# Get ArgoCD Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

---

# Project Outcome

Successfully implemented complete DevOps CI/CD + GitOps pipeline with:

- Automated Docker image build
- Automated image tagging
- Automated DockerHub push
- Automated Helm chart update
- Automated Git commit
- Automated Kubernetes deployment using ArgoCD
- Self-healing GitOps deployment strategy
- Kubernetes monitoring using Prometheus
- Dashboard visualization using Grafana

---

# Future Enhancements

- Loki Logging
- Terraform Infrastructure Automation
- Jenkins Integration
- SonarQube
- Trivy Security Scanning

---

# Screenshots

## GitHub Actions Pipeline

(Add GitHub Actions success screenshot here)

---

## ArgoCD Synced Application

(Add ArgoCD synced screenshot here)

---

## Kubernetes Deployment

(Add Kubernetes deployment screenshot here)

---

## Monitoring Dashboard

(Add Grafana dashboard screenshot here)

---

# Author

Akshay Marathe

GitHub Repository:

https://github.com/aksahram/kubernetes-project

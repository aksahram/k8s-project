🚀 Kubernetes CI/CD Project – Oggy Web Application

📌 Project Overview

This project demonstrates an end-to-end DevOps workflow for deploying a containerized web application using Kubernetes. It includes CI pipeline automation using GitHub Actions and manual CD using Kubernetes rollout strategies.

The application is a simple Nginx-based web server serving a static HTML page.

---

🛠️ Tech Stack

- Containerization: Docker
- Orchestration: Kubernetes
- CI Tool: GitHub Actions
- Web Server: Nginx (Alpine)
- Version Control: GitHub

---

📂 Project Structure

k8s-project/
│── Dockerfile
│── index.html
│── deployment.yaml
│── service.yaml
│── cm.yaml
│── secret.yaml
│── .github/workflows/main.yml

---

⚙️ How It Works (CI/CD Flow)

1. Developer updates application code ("index.html")
2. Code is pushed to GitHub repository
3. GitHub Actions pipeline is triggered
4. Docker image is built automatically
5. Kubernetes deployment is updated manually (due to restricted environment)
6. Rolling update is performed on pods
7. Application is served via NodePort

---

🔄 CI Pipeline (GitHub Actions)

- Trigger: Push to "main" branch
- Steps:
  - Checkout code
  - Build Docker image

«Note: Image push to registry is skipped due to firewall restrictions.»

---

🚀 Kubernetes Deployment

Deployment Features:

- Replica count: 2
- Rolling updates enabled
- Uses ConfigMap and Secret
- Container port: 80

Service:

- Type: NodePort
- Port: 30007

---

🧪 Verification Steps

Check Pods

kubectl get pods -n oggy

Check Service

kubectl get svc -n oggy

Access Application

http://<NODE-IP>:30007

Check Logs

kubectl logs <pod-name> -n oggy

---

🐞 Challenges Faced & Solutions

1. Internet Restricted Environment

- Unable to download external resources (Prometheus, etc.)
- Solution: Worked with local files and manual deployment

2. Missing curl in Container

- Issue: "curl not found"
- Solution: Used "wget" for testing

3. No Logs Showing

- Issue: Logs were empty
- Reason: No incoming traffic
- Solution: Generated traffic using internal requests

---

📊 Monitoring Approach

Due to restrictions:

- Used "kubectl logs" for monitoring
- Used "kubectl describe" for debugging
- Understood Prometheus & Grafana architecture conceptually

---

💡 Key Learnings

- Kubernetes deployment & service management
- Rolling updates and rollback strategies
- CI pipeline using GitHub Actions
- Debugging containers without standard tools
- Handling restricted production-like environments

---

🎯 Future Improvements

- Integrate DockerHub / ECR for image storage
- Automate CD pipeline
- Add Prometheus & Grafana monitoring
- Implement Helm charts

---

👨‍💻 Author

Akshay Marathe
DevOps Engineer l.

---

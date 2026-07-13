# 🚀 End-to-End DevSecOps CI/CD Pipeline for Dify on AWS

> **Project Type:** Hands-on DevSecOps Implementation  
> **Cloud Platform:** AWS EC2  
> **CI/CD Tool:** Jenkins  
> **Container Platform:** Docker & Kubernetes (Minikube)

---

# 📖 Project Overview

This project demonstrates the implementation of a complete **DevSecOps CI/CD Pipeline** for deploying the **Dify AI Application** on AWS.

The objective of this project was to configure an end-to-end DevSecOps environment by integrating Jenkins, SonarQube, Trivy, Docker, Kubernetes (Minikube), and Helm for automated application deployment.

> **Note:**  
> The Dify application source code was obtained from a fork of the original project used for learning purposes. This repository documents my complete DevSecOps implementation, infrastructure setup, CI/CD pipeline configuration, integrations, troubleshooting, and deployment process.

---

# 🏗️ Architecture

(Add Architecture Diagram Here)

Example Flow:

GitHub Repository
        │
        ▼
Jenkins Pipeline
        │
        ├─────────────► Docker Hub Authentication
        │
        ├─────────────► SonarQube Code Analysis
        │
        ├─────────────► Trivy Security Scan
        │
        ├─────────────► Kubernetes Namespace Validation
        │
        ├─────────────► Helm Deployment
        ▼
Kubernetes (Minikube)
        │
        ▼
Dify Application

---

# 🛠️ Technology Stack

| Category | Technology |
|------------|----------------|
| Cloud | AWS EC2 |
| Operating System | Ubuntu |
| CI/CD | Jenkins |
| Containerization | Docker |
| Container Registry | Docker Hub |
| Code Quality | SonarQube |
| Security Scanner | Trivy |
| Container Orchestration | Kubernetes (Minikube) |
| Package Manager | Helm |
| Version Control | Git & GitHub |

---

# ☁️ AWS Infrastructure

- EC2 Instance
- Ubuntu Server
- Instance Type: **t2.large**
- Root Volume: **29 GB**

---

# ⚙️ Project Setup

## Step 1 - Launch AWS EC2 Instance

- Launch Ubuntu EC2 Instance
- Instance Type: **t2.large**
- Root Volume: **29 GB**
- Configure Security Groups
- Connect using SSH

---

## Step 2 - Install Java

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre
java -version
```

---

## Step 3 - Install Jenkins

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins
```

---

## Step 4 - Configure Jenkins Port

Changed default Jenkins port from **8080** to **2000**

Updated

```
/etc/default/jenkins
```

```
HTTP_PORT=2000
```

Updated

```
/lib/systemd/system/jenkins.service
```

```
Environment="JENKINS_PORT=2000"
```

Reload Jenkins

```bash
sudo systemctl daemon-reload
sudo systemctl restart jenkins
```

Access Jenkins

```
http://<EC2-PUBLIC-IP>:2000
```

---

## Step 5 - Install Docker, Kubernetes & Helm

Installed

- Docker
- Kubectl
- Helm
- Docker Compose

```bash
./cluster.sh
```

Configured Docker permissions

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Started Minikube

```bash
minikube start
```

---

## Step 6 - Install Trivy

```bash
wget https://github.com/aquasecurity/trivy/releases/download/v0.69.3/trivy_0.69.3_Linux-64bit.deb

sudo dpkg -i trivy_0.69.3_Linux-64bit.deb
```

---

## Step 7 - Install SonarQube

```bash
docker run -itd \
--name sonarqube-server \
-p 1234:9000 \
sonarqube:lts-community
```

Access SonarQube

```
http://<EC2-IP>:1234
```

---

# 🔧 Jenkins Configuration

Configured

- Jenkins Plugins
- SonarQube Integration
- DockerHub Integration
- Kubernetes Integration

Installed Plugins

- SonarQube Scanner
- Sonar Quality Gates
- Docker
- Kubernetes

---

# 🔑 SonarQube Integration

Configured

- SonarQube Server
- Authentication Token
- Jenkins Credentials
- Sonar Scanner Tool
- SonarQube Webhook

Webhook

```
http://<JENKINS-IP>:2000/sonarqube-webhook/
```

---

# 🐳 Docker Hub Integration

Created

- Docker Hub Personal Access Token

Configured Jenkins Credential

Credential Type

```
Username with Password
```

Credential ID

```
dockerhub-cred-id
```

---

# ☸ Kubernetes Integration

Configured

- Minikube Cluster
- Kubernetes Namespace
- Service Account
- RBAC
- Jenkins Cloud
- Kubernetes Token
- kubeconfig
- Jenkins Kubernetes Authentication

Namespaces

```
dev

prod
```

---

# 🔄 CI/CD Pipeline Flow

```
Code Checkout

        ↓

Docker Hub Login

        ↓

SonarQube Code Analysis

        ↓

Trivy File System Scan

        ↓

Namespace Validation

        ↓

Helm Deployment

        ↓

Deploy Application to Kubernetes
```

---

# 📷 Screenshots

Added screenshots for

- AWS EC2 Instance
- SSH Connection
- Java Installation
- Jenkins Dashboard
- Jenkins Plugins
- SonarQube Dashboard
- DockerHub Credentials
- Kubernetes Cluster
- Pipeline Success
- Running Pods
- Dify Application

---

# 🛠️ Challenges Faced

During this implementation I encountered and resolved several real-world DevSecOps issues:

- SSH private key permission issues
- Jenkins installation and service configuration
- Jenkins port configuration
- Docker permission issues
- Docker Hub authentication using Personal Access Token
- Jenkins credential configuration
- SonarQube integration
- Kubernetes RBAC configuration
- Jenkins to Kubernetes authentication
- kubeconfig permission issues
- Reverse proxy warning in Jenkins

---

# 🎯 Learning Outcomes

This project helped me gain practical hands-on experience with:

- AWS EC2 Administration
- Linux Server Management
- Jenkins Administration
- CI/CD Pipeline Creation
- Docker
- Kubernetes
- Helm
- SonarQube
- Trivy
- GitHub
- Docker Hub
- Jenkins Credentials
- DevSecOps Best Practices
- Troubleshooting Production-like DevOps Issues

---

# ✅ Final Result

Successfully configured and deployed a complete **DevSecOps CI/CD Pipeline** on AWS for the Dify application with:

- Automated Source Code Checkout
- Docker Hub Authentication
- Static Code Analysis
- Security Scanning
- Kubernetes Deployment
- Helm Release Management

---

# 🙏 Acknowledgement

This implementation was completed as part of a hands-on DevSecOps learning project.

The application source code was forked from the original repository provided for learning purposes. The infrastructure setup, Jenkins configuration, integrations, CI/CD pipeline implementation, deployment, debugging, and troubleshooting documented in this repository were performed by me in my own AWS environment.

---

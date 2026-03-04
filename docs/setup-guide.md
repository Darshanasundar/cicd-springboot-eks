# CI/CD Pipeline for Spring Boot Application on AWS EKS
## Setup & Deployment Guide

---

## 👨‍💻 Author

Name: Darshana S  
GitHub: https://github.com/Darshanasundar  

This document explains the complete setup and deployment process used to build and deploy a Spring Boot application using a CI/CD pipeline on AWS EKS.

---

## 📌 Project Objective

To design and implement an automated CI/CD pipeline that:

- Builds a Spring Boot application
- Creates Docker images
- Pushes images to DockerHub
- Deploys applications on AWS EKS
- Uses ArgoCD for continuous deployment

---

## 🏗 Architecture Overview

Pipeline Flow:

GitHub → Jenkins → Maven → Docker → DockerHub → ArgoCD → AWS EKS

---

## ⚙️ Prerequisites

- AWS Account with Admin Access
- GitHub Account
- DockerHub Account
- Windows/Mac Machine
- Internet Connection

---

## 🖥 Local Machine Setup

### Install AWS CLI

Download and install:

https://awscli.amazonaws.com/AWSCLIV2.msi

Configure AWS:

```bash
aws configure
```
---

### Install Kubernetes Tools

```bash
choco install kubernetes-cli -y
choco install eksctl -y
```

Verify:

```bash
kubectl version --client
eksctl version
```

---

## ☁️ Create AWS EKS Cluster

```bash
eksctl create cluster \
--name cicddemo \
--node-type m7i-flex.large \
--nodes 1 \
--nodes-min 1 \
--nodes-max 1 \
--region eu-west-2 \
--zones eu-west-2a,eu-west-2b
```

---

## 🖥 Jenkins Server Setup (EC2)

### Launch EC2

- Ubuntu 22.04
- 8 GB RAM
- Allow All Traffic (For Learning Only)

---

### Install Java

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
sudo apt install openjdk-21-jdk -y
```

---

### Install Jenkins

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian/jenkins.io-2026.key
```

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list
```

```bash
sudo apt update
sudo apt install jenkins -y
```

---

### Install Required Tools

```bash
sudo apt install git docker.io maven -y
```

---

### Configure Jenkins User

```bash
sudo usermod -aG docker jenkins
sudo su - jenkins
```

---

### Configure Environment Variables

Edit:

```bash
vi ~/.bashrc
```

Add:

```bash
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
export MAVEN_HOME=/usr/share/maven
export PATH=$PATH:$MAVEN_HOME/bin
```

Reload:

```bash
source ~/.bashrc
```

---

## 🔐 GitHub SSH Configuration

Generate SSH Key:

```bash
sudo -u jenkins ssh-keygen -t rsa -b 4096
```

Copy Public Key:

```bash
cat /var/lib/jenkins/.ssh/id_rsa.pub
```

Add it to GitHub → Settings → SSH Keys.

---

## 🔑 Jenkins Credentials Setup

### GitHub SSH Credential

- ID: git-ssh-cred
- Type: SSH Username with Private Key

### DockerHub Credential

- ID: docker-secret
- Username: DockerHub Username
- Password: DockerHub Password

---

## ⚙️ Jenkins Pipeline Configuration

Create Pipeline Job → Add Pipeline Script.

Main Stages:

### Stage 1: Checkout
- Clone application repository
- Clone Jenkins repository

### Stage 2: Build
- Run Maven build
- Generate JAR file

### Stage 3: Docker Build & Push
- Build Docker image
- Push to DockerHub

### Stage 4: Update Manifest
- Update Kubernetes YAML
- Push changes to GitHub

---

## 🐳 Docker Configuration

Dockerfile Base Image:

```dockerfile
FROM eclipse-temurin:21-jdk
```

Build:

```bash
docker build -t springapi .
```

---

## ☸️ ArgoCD Installation

Update kubeconfig:

```bash
aws eks update-kubeconfig --name cicddemo --region eu-west-2
```

Create Namespace:

```bash
kubectl create namespace argocd
```

Install ArgoCD:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Port Forward:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

---

### Get ArgoCD Password

```powershell
kubectl -n argocd get secret argocd-initial-admin-secret `
-o jsonpath="{.data.password}" | ForEach-Object {
[System.Text.Encoding]::UTF8.GetString(
[System.Convert]::FromBase64String($_))
}
```

---

## 🚀 Application Deployment

1. Login to ArgoCD UI
2. Create Application
3. Connect Jenkins Repository
4. Enable Auto Sync
5. Deploy to EKS

Verify:

```bash
kubectl get pods
kubectl get svc
```

---

## 🧹 Resource Cleanup

Delete EKS Cluster:

```bash
eksctl delete cluster --name cicddemo --region eu-west-2
```

Terminate EC2 instances from AWS Console.

---

## 📚 Key Learnings

- CI/CD automation
- Jenkins pipeline design
- Containerization
- Kubernetes deployment
- GitOps using ArgoCD
- AWS infrastructure management

---

## ✅ Conclusion

This project demonstrates a complete DevOps workflow using industry-standard tools to automate application deployment on AWS.

All configurations, deployments, testing, and documentation were performed as part of hands-on learning.

---

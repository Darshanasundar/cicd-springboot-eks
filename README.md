# CI/CD Pipeline for Spring Boot Application on AWS EKS

## 👨‍💻 Author
Darshana S  
GitHub: https://github.com/Darshanasundar

## 📌 Overview
This project implements an end-to-end CI/CD pipeline for a Spring Boot application deployed on AWS EKS using Jenkins, Docker, and ArgoCD.

## 🏗 Architecture
GitHub → Jenkins → Maven → Docker → DockerHub → ArgoCD → EKS

## ⚙️ Technologies
- Java (Spring Boot)
- Jenkins
- Docker
- Kubernetes (EKS)
- ArgoCD
- AWS
- GitHub

## 🚀 Pipeline Flow
1. Code pushed to GitHub
2. Jenkins triggers build
3. Maven builds JAR
4. Docker image created
5. Image pushed to DockerHub
6. Manifest updated
7. ArgoCD deploys to EKS

## 📁 Structure

cicd-springboot-eks/
├── app/
├── jenkins/
├─k8s/
├─docs/
└─README.md


## 📄 Documentation
See docs/setup-guide.md for full setup.

## 📚 Learnings
- CI/CD automation
- Kubernetes deployment
- DevOps pipeline design
- Cloud-native deployment

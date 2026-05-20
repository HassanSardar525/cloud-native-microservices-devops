# ShopCloud — Cloud-Native Microservices DevOps Project

## Overview
A cloud-native e-commerce platform built with microservices architecture,
containerized with Docker, orchestrated with Kubernetes, and deployed via
automated CI/CD pipelines. Built as a final project for the DevOps for
Cloud Computing course at COMSATS University Islamabad, Lahore Campus.

---

## Architecture

The platform consists of 5 independent microservices:

| Service              | Description                              | Port |
|----------------------|------------------------------------------|------|
| Frontend             | Main dashboard and navigation hub        | 8080 |
| User Service         | Registration, login, JWT authentication  | 8081 |
| Product Service      | Product catalog, inventory, CRUD APIs    | 8082 |
| Order Service        | Order processing and status tracking     | 8083 |
| Notification Service | Email and in-app alerts                  | 8084 |

---

## Tech Stack

- **Frontend**: Static HTML + CSS
- **Web Server**: nginx:alpine
- **Containerization**: Docker + Docker Compose
- **Orchestration**: Kubernetes
- **CI/CD**: GitHub Actions + Jenkins
- **Registry**: Docker Hub
- **Version Control**: Git Flow

---

## Environments & Branches

| Branch  | Environment | CI Pipeline        | CD Pipeline        |
|---------|-------------|--------------------|--------------------|
| develop | Development | ci-development.yml | cd-development.yml |
| staging | Staging     | ci-staging.yml     | cd-staging.yml     |
| main    | Production  | ci-production.yml  | cd-production.yml  |

---

## Getting Started

### Prerequisites
- Docker Desktop installed
- Git installed

### Run locally with Docker Compose
```bash
git clone https://github.com/HassanSardar525/cloud-native-microservices-devops.git
cd cloud-native-microservices-devops
docker compose up --build
```

Then open:
- http://localhost:8080 — Frontend dashboard
- http://localhost:8081 — User Service
- http://localhost:8082 — Product Service
- http://localhost:8083 — Order Service
- http://localhost:8084 — Notification Service

---

## Docker Hub

All images are available at:
- hassansardar525/frontend:1.0.0
- hassansardar525/user-service:1.0.0
- hassansardar525/product-service:1.0.0
- hassansardar525/order-service:1.0.0
- hassansardar525/notification-service:1.0.0

---

## CI/CD Pipeline

### GitHub Actions
Each environment has a dedicated CI and CD workflow:
- CI runs on every push — lints HTML, verifies CSS, builds and pushes Docker images
- CD triggers after CI passes — pulls latest images and deploys

### Jenkins Pipeline
Stages: Checkout → Lint & Test → Docker Build → Push → Deploy → Verify → Notify

Includes automatic rollback on failure via `kubectl rollout undo`.

---

## Kubernetes Deployment

Apply all manifests:
```bash
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/frontend-deployment.yaml
kubectl apply -f k8s/user-deployment.yaml
kubectl apply -f k8s/product-deployment.yaml
kubectl apply -f k8s/order-deployment.yaml
kubectl apply -f k8s/notification-deployment.yaml
```

Verify pods are running:
```bash
kubectl get pods
kubectl get services
```

---

## Project Reflection

This project gave us hands-on experience with the full DevOps lifecycle —
from writing Dockerfiles and managing multi-container applications with
Docker Compose, to automating deployments with GitHub Actions and writing
Kubernetes manifests for production-grade orchestration. The biggest
challenge was understanding how isolated Docker containers communicate
and how CI/CD pipelines chain together across multiple environments.

---

## Team

COMSATS University Islamabad, Lahore Campus
Course: Lab-DevOps for Cloud Computing

Members:
Muhammad Hassan : FA23-BCS-205
Talha Ameen     : FA23-BCS-199
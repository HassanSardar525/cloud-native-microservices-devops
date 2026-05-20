# CLAUDE.md — Project Context for Claude Code

## Project Overview
This is a DevOps final project for COMSATS University Islamabad, Lahore Campus.
Course: Lab-DevOps for Cloud Computing (Semester 6, BSCS)

The project simulates an enterprise-level cloud DevOps workflow for a fictional
e-commerce platform called **ShopCloud**, built with microservices architecture.

---

## Project Location
E:\Projects\devops_cloud_project\cloud-native-microservices-devops

---

---

## Tech Stack
- **Frontend**: Static HTML + CSS (no framework)
- **Web Server**: nginx:alpine (inside Docker)
- **Containerization**: Docker + Docker Compose
- **Orchestration**: Kubernetes (k8s/ manifests)
- **CI/CD**: GitHub Actions + Jenkins
- **Registry**: Docker Hub (username: hassansardar525)
- **Deployment**: Render.com (3 environments)
- **Version Control**: Git Flow on GitHub (public repo)

---

## Docker Details

### Port Mapping
| Service              | Local Port | Container Port |
|----------------------|------------|----------------|
| frontend             | 8080       | 80             |
| user-service         | 8081       | 80             |
| product-service      | 8082       | 80             |
| order-service        | 8083       | 80             |
| notification-service | 8084       | 80             |

### Docker Hub Images
- hassansardar525/frontend:1.0.0
- hassansardar525/user-service:1.0.0
- hassansardar525/product-service:1.0.0
- hassansardar525/order-service:1.0.0
- hassansardar525/notification-service:1.0.0

### Docker Compose File: docker-compose.yml
Network name: ecommerce-microservices_ecommerce-net
Network driver: bridge

### Dockerfile pattern (same for all services):
```dockerfile
FROM nginx:alpine
LABEL service="<service-name>"
LABEL maintainer="majid1@live.co.uk"
COPY index.html /usr/share/nginx/html/index.html
COPY style.css /usr/share/nginx/html/style.css
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## Git & Branching Strategy (Git Flow)
- **main** → Production environment
- **staging** → Staging/QA environment  
- **develop** → Active development (default working branch)

### Commit message format (Conventional Commits):
- feat: new feature
- fix: bug fix
- docs: documentation change
- ci: CI/CD pipeline change
- chore: maintenance

### Remote: GitHub (public repo)
Always push to develop unless told otherwise.
Never commit directly to main.

---

## GitHub Environments
Three environments must exist on GitHub:
1. **development** → deploys from develop branch
2. **staging** → deploys from staging branch
3. **production** → deploys from main branch

Secrets are managed per environment (not global).

---

## CI/CD Requirements

### GitHub Actions (Phase 4)
- Separate CI workflow: HTML linting + Docker image build
- Separate CD workflow: deploy to Render.com
- One pair of CI+CD workflows per environment (development, staging, production)
- Workflows live in .github/workflows/
- Triggered by push to the corresponding branch

### Jenkins (Phase 6)
Pipeline stages: Checkout → Build → Test → Docker Build → Push → Deploy → Notify
- Declarative pipeline syntax
- Jenkinsfile lives at project root
- Must include rollback step on failure

---

## Kubernetes (Phase 5)
All manifests live in k8s/ folder.
Each service needs:
- Deployment (2 replicas minimum)
- Service (ClusterIP or NodePort)
- ConfigMap for environment variables
- Secret for sensitive values

---

## Known Issues & Fixes Applied
- style.css must be copied into each service folder individually because
  each service runs in its own isolated Docker container and cannot access
  sibling folders
- Navigation links must use absolute localhost URLs (http://localhost:PORT)
  not relative paths, because relative paths break across containers
- Always run `docker compose build --no-cache` after changing HTML/CSS/Dockerfile
  to prevent Docker from serving stale cached layers

---

## Current Progress
- [x] Phase 1 — Git repo, branches, folder structure
- [x] Phase 2 — HTML/CSS pages for all 5 services
- [x] Phase 3 — Dockerfiles, docker-compose.yml, images built and running
- [ ] Phase 4 — GitHub Actions CI/CD pipelines
- [ ] Phase 5 — Kubernetes manifests
- [ ] Phase 6 — Jenkins pipeline + advanced features

---

## Important Rules for Claude Code
1. Always work from the develop branch unless told otherwise
2. After any file change, run `docker compose build --no-cache && docker compose up -d`
3. After verifying changes work, always commit with a meaningful message and push to develop
4. Never hardcode secrets — use environment variables or GitHub Secrets
5. Keep all 5 services consistent — if you change a pattern in one, apply it to all
6. This project runs on Windows 11 with WSL2 — use cmd-compatible commands
7. Docker Hub username is: hassansardar525
8. Semantic versioning format: 1.0.0 (major.minor.patch)
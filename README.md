cat > README.md << 'EOF'
# Cloud-Native Microservices DevOps Project

## Overview
A cloud-native e-commerce platform with microservices architecture, containerized with Docker, orchestrated with Kubernetes, and deployed via CI/CD pipelines.

## Services
- **Frontend** - Dashboard UI
- **User Service** - Registration, login, authentication
- **Product Service** - Product catalog and inventory
- **Order Service** - Order processing and tracking
- **Notification Service** - Email and in-app alerts

## Environments
| Branch | Environment |
|--------|-------------|
| develop | Development |
| staging | Staging/QA |
| main | Production |

## Setup
Clone the repo and follow per-service instructions in each `src/` subfolder.
EOF
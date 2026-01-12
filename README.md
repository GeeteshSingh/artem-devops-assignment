# DevOps Engineer Assignment – Artem HealthTech
## Overview

This repository demonstrates a hands-on DevOps workflow covering CI/CD, containerization, Kubernetes deployment, and operational thinking for a Spring Boot application.
The focus of this assignment is on **clarity, stability, automation, and cost awareness**, rather than over-engineering.

---
## Tech Stack
- Java 17
- Spring Boot
- Maven
- Docker
- GitHub Actions
- Kubernetes
- MySQL (Docker-based)
- Kafka (Docker-based)
---
## Repository Structure
![img_1.png](img_1.png)
---
## CI/CD Pipeline
The CI/CD pipeline is implemented using GitHub Actions and follows these stages:
1. Source code checkout
2. Java 17 setup
3. Maven build and tests
4. Docker image build
5. Push image to GitHub Container Registry (GHCR)
6. Logical deployment step for Dev environment
The pipeline is designed to fail fast and ensure only validated builds are containerized and promoted.
Pipeline configuration can be found under:
ci/github-actions.yaml
---
## Docker

The application is containerized using a lightweight Java 17 JRE base image to reduce image size and improve startup time.

Dockerfile location:

docker/Dockerfile

---
## Kubernetes Deployment
Kubernetes manifests are provided for **Dev** and **QA** environments with the following characteristics:

- Namespace-based environment isolation
- Deployment, Service, and ConfigMap resources
- CPU and memory requests/limits for cost awareness
- Liveness and readiness probes using Spring Boot Actuator health endpoint

Manifests location:

k8s/dev

k8s/qa

---

## Database & Messaging
For local and demonstration purposes:
- **MySQL** is used as the relational database
- **Kafka** is used for messaging

Both are configured using Docker Compose:
docker-compose.yaml
Configuration is externalized and no secrets are hardcoded.
---

## Stability, Cost & Operational Considerations
All answers related to:
- Environment stability (Dev / QA / Prod)
- Rollback strategy
- Cost optimizations
- Monitoring (pods, Kafka lag, database performance)
- Future automation improvements

are documented in detail here:

➡️ **[Stability & Cost Optimization Document](stability-cost.md)**

This separation keeps the README concise while still providing complete operational reasoning.

---

## For local setup

mvn clean package
docker build -t spring-app .
docker run -p 8080:8080 spring-app

Health check:
curl http://localhost:8080/actuator/health



**Author**

**Geetesh Singh**
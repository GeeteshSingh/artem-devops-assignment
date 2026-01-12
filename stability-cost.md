# Stability, Cost & Operational Considerations

This document outlines the approach taken to ensure application stability, cost efficiency, and operational reliability across environments, along with future automation improvements.

---

## 1. Ensuring Stability Across Dev / QA / Prod

Stability across environments is ensured through the following practices:

- **Environment Isolation**
  - Separate Kubernetes namespaces for Dev, QA, and Prod
  - Prevents configuration leakage and accidental cross-environment impact

- **CI/CD Validation**
  - Maven build and tests executed before containerization
  - Only validated artifacts are promoted to the container registry

- **Health Checks**
  - Liveness and readiness probes using Spring Boot Actuator
  - Ensures traffic is routed only to healthy pods
  - Prevents unstable pods from serving requests

- **Configuration Management**
  - Environment-specific configuration via ConfigMaps
  - No hardcoded environment values inside application code

---

## 2. Rollback Strategy for Failed Deployments

Rollback is handled using native Kubernetes mechanisms:

- **Kubernetes Rollout Undo**
  - Revert to the last stable ReplicaSet using:
    ```
    kubectl rollout undo deployment/spring-app -n <namespace>
    ```

- **Image-Based Rollback**
  - Previous container images remain available in the registry
  - Allows redeployment of a known stable version

This approach enables fast recovery with minimal downtime.

---

## 3. Cost Optimizations Applied

Cost efficiency is addressed using the following measures:

- **Resource Requests & Limits**
  - CPU and memory requests defined to prevent over-provisioning
  - Limits prevent noisy-neighbor issues

- **Replica Strategy**
  - Single replica used in non-production environments
  - Avoids unnecessary infrastructure costs during development and testing

- **Lightweight Container Image**
  - Java JRE base image used instead of full JDK
  - Reduces image size and startup time

---

## 4. Monitoring Strategy

### Pod Health
- Kubernetes liveness and readiness probes
- Metrics collection via Kubernetes metrics server or Prometheus

### Kafka Lag
- Consumer group lag monitoring
- Kafka metrics via JMX exporter and Prometheus
- Alerts on abnormal lag growth

### Database Performance
- Slow query logs enabled for MySQL
- Database metrics such as connection count and query latency
- Monitoring via database metrics exporters

---

## 5. Future Automation & Improvements

If given more time, the following would be automated or enhanced:

- **Horizontal Pod Autoscaler (HPA)**
  - Automatic scaling based on CPU or memory utilization

- **Secrets Management**
  - Replace environment variables with Kubernetes Secrets or external secret managers

- **GitOps Deployment**
  - Declarative deployments using tools like ArgoCD or Flux
  - Improved traceability and rollback control

- **Centralized Monitoring & Alerting**
  - Prometheus and Grafana dashboards
  - Alerting via Alertmanager for proactive incident response

---

## Summary

The solution prioritizes stability, cost awareness, and operational clarity.  
The design choices reflect real-world DevOps practices while keeping the implementation simple, transparent, and maintainable within the assignment scope.

# 04-deployments

## Overview
Kubernetes Deployments are a resource that manages a set of Pods to ensure scalability, high availability, and seamless updates. Deployments use a declarative approach to define the desired state of an application, automatically managing Pod creation, scaling, updates, and rollbacks. Unlike standalone Pods, Deployments are designed for stateless applications, providing features like rolling updates, replica management, and self-healing. Deployments work with ReplicaSets to maintain the desired number of Pods and are a cornerstone for running production-grade applications in Kubernetes.

### What is a Kubernetes Deployment?
A Deployment is a Kubernetes resource that defines a desired state for Pods, including the number of replicas, container specifications, and update strategies. It ensures that the specified number of Pods are running and healthy, automatically replacing failed Pods and managing updates. Deployments are typically used for stateless applications (e.g., web servers, APIs), while StatefulSets (covered in `11-daemonsets-statefulsets/`) are used for stateful applications.

### Deployment Configurations
Kubernetes Deployments support several key configurations and use cases:

1. **Basic Deployment**
    - **Definition**: A Deployment that manages a set of identical Pods with a specified number of replicas.
    - **Use Case**: Running a stateless application with multiple instances for redundancy and load balancing.
    - **When to Use**: When you need to deploy a simple application with a fixed number of Pods, such as a web server.
    - **Example**: A Deployment running three replicas of an NGINX web server.
    - **Key Features**:
        - Ensures the desired number of Pods are running.
        - Supports basic scaling and self-healing.
        - Works with Services for load balancing.

2. **Scaling Deployment**
    - **Definition**: A Deployment that adjusts the number of Pod replicas to handle varying loads.
    - **Use Case**: Scaling an application up or down based on demand, such as during traffic spikes.
    - **When to Use**: When you need to dynamically adjust the number of Pods to match workload requirements.
    - **Example**: Scaling an API Deployment from 3 to 5 replicas during peak hours.
    - **Key Features**:
        - Manual scaling via `kubectl scale` or declarative updates to the Deployment YAML.
        - Supports Horizontal Pod Autoscaler (HPA) for automatic scaling (covered in `12-resource-management/`).
        - Maintains high availability during scaling.

3. **Rolling Update**
    - **Definition**: A Deployment update strategy that incrementally replaces old Pods with new ones to minimize downtime.
    - **Use Case**: Updating an application to a new version (e.g., new container image) without interrupting service.
    - **When to Use**: When you need zero-downtime updates for production applications.
    - **Example**: Updating an NGINX Deployment from version 1.20 to 1.21.
    - **Key Features**:
        - Configurable via `strategy` field (e.g., `maxSurge`, `maxUnavailable`).
        - Ensures gradual replacement of Pods.
        - Supports health checks to verify new Pods are ready.

4. **Rollback Deployment**
    - **Definition**: Reverting a Deployment to a previous version if an update fails or introduces issues.
    - **Use Case**: Recovering from a faulty deployment, such as a buggy application version.
    - **When to Use**: When you need to quickly revert to a stable state after a failed update.
    - **Example**: Rolling back a Deployment to a previous NGINX version after a failed update.
    - **Key Features**:
        - Tracks revision history via ReplicaSets.
        - Supports `kubectl rollback` or `kubectl apply` with previous manifests.
        - Configurable history limit via `revisionHistoryLimit`.

### Best Practices for Kubernetes Deployments
To ensure reliable, scalable, and maintainable Deployments, follow these best practices:

1. **Use Deployments for Stateless Applications**:
    - Prefer Deployments over standalone Pods for stateless applications to leverage scaling, updates, and self-healing.
    - Example: Use a Deployment for a REST API instead of a standalone Pod.

2. **Define Clear Labels and Selectors**:
    - Use consistent labels (e.g., `app: my-app`, `env: prod`) and matchLabels to ensure Deployments target the correct Pods.
    - Example: `selector: { matchLabels: { app: nginx } }`.

3. **Configure Health Checks**:
    - Include liveness and readiness probes in Pod templates to ensure only healthy Pods receive traffic.
    - Example:
      ```yaml
      livenessProbe:
        httpGet:
          path: /healthz
          port: 80
        initialDelaySeconds: 15
        periodSeconds: 10
      ```

4. **Set Resource Requests and Limits**:
    - Define CPU and memory requests and limits to optimize resource usage and prevent overconsumption.
    - Example:
      ```yaml
      resources:
        requests:
          cpu: "100m"
          memory: "128Mi"
        limits:
          cpu: "500m"
          memory: "512Mi"
      ```

5. **Use Rolling Update Strategy**:
    - Configure `strategy: rollingUpdate` with appropriate `maxSurge` and `maxUnavailable` settings to ensure zero-downtime updates.
    - Example: `maxSurge: 25%, maxUnavailable: 25%` allows 25% additional Pods while keeping 75% available.

6. **Limit Revision History**:
    - Set `revisionHistoryLimit` to control the number of old ReplicaSets retained for rollbacks (e.g., 10) to save cluster resources.
    - Example: `revisionHistoryLimit: 5`.

7. **Pair with Services**:
    - Use a Service (e.g., ClusterIP) to provide a stable endpoint for accessing Pods managed by the Deployment.
    - Example: Create a Service with `selector: { app: nginx }` to load balance traffic to Deployment Pods.

8. **Enable Autoscaling**:
    - Use Horizontal Pod Autoscaler (HPA) to automatically scale Deployments based on CPU, memory, or custom metrics (covered in `12-resource-management/`).
    - Example: `kubectl autoscale deployment my-app --min=2 --max=10 --cpu-percent=80`.

9. **Secure Deployments**:
    - Run containers as non-root users and apply Pod Security Standards (covered in `21-security-hardening/`).
    - Use RBAC to restrict Deployment modifications (covered in `13-rbac/`).
    - Example: `securityContext: { runAsNonRoot: true }`.

10. **Monitor and Troubleshoot**:
    - Monitor Deployment status with `kubectl describe deployment` and `kubectl rollout status`.
    - Integrate with monitoring tools like Prometheus (covered in `18-monitoring-logging/`) to track performance.
    - Check Pod logs and events for debugging: `kubectl logs <pod-name>`.

### When to Use Each Deployment Configuration
| **Configuration** | **When to Use** | **Example Scenario** |
|--------------------|-----------------|----------------------|
| **Basic Deployment** | Deploying a stateless application with a fixed number of replicas. | Running a web server with three NGINX replicas. |
| **Scaling Deployment** | Adjusting replicas to handle varying traffic or load. | Scaling an API from 3 to 5 replicas during a traffic spike. |
| **Rolling Update** | Updating an application to a new version with zero downtime. | Updating a web app from version 1.0 to 1.1. |
| **Rollback Deployment** | Reverting to a previous version after a failed update. | Rolling back a buggy API version to a stable one. |

## Exercises
This directory contains hands-on exercises to learn about Kubernetes Deployments:

1. **01-basic-deployment**: Create and manage a simple Deployment with NGINX Pods.
2. **02-scaling-deployment**: Scale a Deployment manually and observe replica management.
3. **03-rolling-update**: Perform a rolling update to a new container image version.
4. **04-rollback-deployment**: Roll back a Deployment to a previous version.
5. **05-troubleshooting**: Diagnose and fix issues with a misconfigured Deployment.

## Prerequisites
- A Kubernetes cluster (e.g., Minikube, Kind, or Docker Desktop) set up from `00-setup/`.
- `kubectl` installed and configured.
- Familiarity with Pods and Services from `02-pods/` and `03-services/`.

## Getting Started
Navigate to each exercise directory and follow the instructions in `exercise.md`. Use the provided `Makefile` to apply or delete resources.

Example:
```bash
cd 01-basic-deployment
make apply
kubectl get deployments
make delete
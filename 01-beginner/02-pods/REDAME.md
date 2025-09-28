# 02-pods

## Overview
Kubernetes Pods are the smallest deployable units in Kubernetes, representing one or more containers that share storage, networking, and a specification for how to run. Pods are designed to run multiple containers that need to work together and share resources, such as a web server and a sidecar logging container. Each Pod has a unique IP address within the cluster, and containers within a Pod can communicate with each other using `localhost`. Pods are ephemeral by nature, often managed by higher-level controllers like Deployments or StatefulSets, but understanding Pods is fundamental to mastering Kubernetes.

### What is a Kubernetes Pod?
A Pod is a Kubernetes resource that encapsulates one or more containers, along with shared storage (volumes), network (unique IP), and configuration (e.g., environment variables, commands). Pods are designed to run a single instance of an application or a tightly coupled group of containers that share a lifecycle. For example, a Pod might run a web server container and a sidecar container that syncs data to an external service.

### Pod Configurations
Kubernetes supports several Pod configurations, each suited for specific use cases:

1. **Single-Container Pod**
    - **Definition**: A Pod containing one container, the simplest and most common Pod configuration.
    - **Use Case**: Running a single application or service, such as a web server (e.g., NGINX) or a standalone API.
    - **When to Use**: When your application consists of a single process that doesn’t require additional containers for auxiliary tasks.
    - **Example**: A Pod running an NGINX web server to serve static content.
    - **Key Features**:
        - Simplest Pod configuration, easy to manage.
        - Suitable for stateless applications.
        - Managed by controllers like Deployments for scaling and updates.

2. **Multi-Container Pod**
    - **Definition**: A Pod containing multiple containers that share resources (e.g., volumes, network) and work together.
    - **Use Case**: Running tightly coupled processes, such as a web server with a logging sidecar or a data producer with a data processor.
    - **When to Use**: When containers need to share data or communicate locally (via `localhost`) for tasks like logging, monitoring, or data synchronization.
    - **Example**: A Pod with an NGINX container and a sidecar container that pushes logs to a centralized system.
    - **Key Features**:
        - Containers share the same network namespace (same IP and port space).
        - Containers can share volumes for data exchange.
        - All containers are scheduled on the same node and share a lifecycle.

3. **Pods with Health Checks**
    - **Definition**: Pods configured with liveness and readiness probes to monitor container health and readiness to serve traffic.
    - **Use Case**: Ensuring applications are healthy and ready to handle requests, preventing traffic from being sent to unhealthy containers.
    - **When to Use**: For production applications where reliability is critical, such as APIs or web services.
    - **Example**: A Pod with an NGINX container that uses HTTP probes to check `/healthz` for liveness and `/index.html` for readiness.
    - **Key Features**:
        - Liveness probes detect when a container needs to be restarted (e.g., if it crashes or becomes unresponsive).
        - Readiness probes determine when a container is ready to accept traffic.
        - Supports HTTP, TCP, or command-based probes.

### Best Practices for Kubernetes Pods
To ensure reliable, scalable, and maintainable Pods, follow these best practices:

1. **Use Controllers for Managing Pods**:
    - Avoid running standalone Pods in production; instead, use controllers like Deployments, StatefulSets, or Jobs to manage Pods for scalability and resilience.
    - Example: Use a Deployment to manage multiple replicas of a web server Pod.

2. **Define Clear Labels**:
    - Assign meaningful labels to Pods (e.g., `app: my-app`, `env: prod`) to facilitate Service routing, monitoring, and management.
    - Example: `labels: { app: nginx, tier: frontend }`.

3. **Configure Health Checks**:
    - Always define liveness and readiness probes for production Pods to ensure Kubernetes can detect and recover from failures.
    - Example:
      ```yaml
      livenessProbe:
        httpGet:
          path: /healthz
          port: 80
        initialDelaySeconds: 15
        periodSeconds: 10
      ```

4. **Limit Resource Usage**:
    - Set resource requests and limits (CPU, memory) to prevent Pods from consuming excessive cluster resources and to ensure fair scheduling.
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

5. **Use Multi-Container Pods Judiciously**:
    - Only use multi-container Pods when containers are tightly coupled and share a lifecycle (e.g., a main app and a logging sidecar).
    - Avoid overloading Pods with unrelated containers; use separate Pods for independent services.

6. **Leverage Init Containers**:
    - Use init containers for setup tasks that must complete before the main containers start (e.g., initializing a database schema).
    - Example: An init container that checks if a database is ready before starting the app.

7. **Manage Storage with Volumes**:
    - Use volumes for shared storage in multi-container Pods or for persistent data (covered in `06-volumes/`).
    - Example: A shared `emptyDir` volume for a web server and logging sidecar.

8. **Enable Logging and Monitoring**:
    - Ensure containers log to `stdout`/`stderr` for Kubernetes to capture logs via `kubectl logs`.
    - Integrate with monitoring tools (covered in `18-monitoring-logging/`) to track Pod health and performance.

9. **Secure Pods**:
    - Run containers as non-root users when possible to reduce security risks.
    - Use Pod Security Policies or PodSecurityStandards (covered in `21-security-hardening/`) to enforce security constraints.
    - Example: `securityContext: { runAsNonRoot: true }`.

10. **Test and Troubleshoot Pods**:
    - Regularly inspect Pod status with `kubectl describe pod <pod-name>` and logs with `kubectl logs <pod-name>`.
    - Use `kubectl exec` to debug containers interactively (e.g., `kubectl exec -it <pod-name> -- /bin/sh`).

### When to Use Each Pod Configuration
| **Pod Configuration** | **When to Use** | **Example Scenario** |
|-----------------------|-----------------|----------------------|
| **Single-Container Pod** | Running a standalone application or service that doesn’t require auxiliary containers. | A simple NGINX web server serving static content. |
| **Multi-Container Pod** | Running tightly coupled containers that share resources or communicate locally. | A web app with a sidecar that syncs data to an external storage service. |
| **Pods with Health Checks** | Ensuring reliability for production applications by detecting and recovering from failures. | A REST API with liveness and readiness probes to ensure uptime. |

## Exercises
This directory contains hands-on exercises to learn about Kubernetes Pods:

1. **01-basic-pod**: Create and manage a simple Pod running an NGINX container.
2. **02-pod-labels**: Use labels to organize and select Pods.
3. **03-multi-container-pod**: Run multiple containers in a single Pod and explore their interactions.
4. **04-health-checks**: Configure liveness and readiness probes for Pods.
5. **05-troubleshooting**: Diagnose and fix issues with a misconfigured Pod.

## Prerequisites
- A Kubernetes cluster (e.g., Minikube, Kind, or Docker Desktop) set up from `00-setup/`.
- `kubectl` installed and configured.
- Basic understanding of Kubernetes architecture from `01-introduction/`.

## Getting Started
Navigate to each exercise directory and follow the instructions in `exercise.md`. Use the provided `Makefile` to apply or delete resources.

Example:
```bash
cd 01-basic-pod
make apply
kubectl get pods
make delete
```
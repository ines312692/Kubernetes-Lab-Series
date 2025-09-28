# 03-services

## Overview
Kubernetes Services provide a stable networking endpoint to access one or more Pods, enabling communication within a cluster and, when needed, externally. A Service abstracts the dynamic nature of Pods, which can be created, destroyed, or rescheduled, by providing a consistent IP address or DNS name. Services enable load balancing, service discovery, and connectivity to applications running in Pods, making them essential for building scalable and reliable applications in Kubernetes.

### What is a Kubernetes Service?
A Service is a Kubernetes resource that defines a logical set of Pods and a policy for accessing them. It uses labels and selectors to identify the Pods it targets and provides a single endpoint (IP or DNS name) for clients to access those Pods. Services decouple the application frontend from the backend, allowing Pods to scale or move without affecting clients.

### Service Types
Kubernetes supports four main Service types, each suited for specific use cases:

1. **ClusterIP** (Default)
  - **Definition**: Exposes the Service on an internal cluster IP, accessible only within the cluster.
  - **Use Case**: Internal communication between microservices or components within the cluster (e.g., a frontend Pod accessing a backend API).
  - **When to Use**: When you need to expose a service internally to other Pods in the cluster, such as for a database or internal API.
  - **Example**: A web application’s backend Pods accessed by frontend Pods via a ClusterIP Service.
  - **Key Features**:
    - Provides a stable virtual IP (ClusterIP) for internal access.
    - Supports load balancing across multiple Pods.
    - Discovered via environment variables or DNS.

2. **NodePort**
  - **Definition**: Exposes the Service on each node’s IP at a specific port (30000–32767 by default), accessible from outside the cluster.
  - **Use Case**: Testing or temporary external access to a service, typically in development or on-premises environments.
  - **When to Use**: When you need simple external access to a service without relying on a cloud provider’s load balancer, or for debugging purposes.
  - **Example**: Accessing a web application directly on a node’s IP during development.
  - **Key Features**:
    - Exposes a high-numbered port on all cluster nodes.
    - Forwards traffic to the selected Pods.
    - Can be used with a single node (e.g., Minikube) or multiple nodes.

3. **LoadBalancer**
  - **Definition**: Exposes the Service externally using a cloud provider’s load balancer, which assigns an external IP address.
  - **Use Case**: Production-grade external access to applications in cloud environments (e.g., AWS ELB, GCP Cloud Load Balancer).
  - **When to Use**: When you need reliable, scalable external access to a service in a cloud environment, such as a public-facing web application.
  - **Example**: A customer-facing API exposed via an AWS Elastic Load Balancer.
  - **Key Features**:
    - Integrates with cloud provider load balancers.
    - Provides a single external IP for access.
    - Supports advanced load balancing features (e.g., SSL termination, depending on the provider).

4. **ExternalName**
  - **Definition**: Maps a Service to an external DNS name without creating a ClusterIP or proxying traffic.
  - **Use Case**: Accessing external services (outside the cluster) as if they were internal Kubernetes Services, useful for integrating with third-party APIs or external databases.
  - **When to Use**: When you want to reference an external service (e.g., a SaaS provider’s API) using Kubernetes DNS without proxying traffic through the cluster.
  - **Example**: Redirecting requests to an external database like `db.example.com`.
  - **Key Features**:
    - No Pods or ClusterIP are created; it’s a DNS alias.
    - Simplifies integration with external resources.
    - Requires no cluster resources beyond the Service object.

### Best Practices for Kubernetes Services
To ensure reliable, scalable, and maintainable Services, follow these best practices:

1. **Use Meaningful Labels and Selectors**:
  - Define clear and consistent labels on Pods (e.g., `app: my-app`, `env: prod`) to ensure Services target the correct Pods.
  - Use specific selectors to avoid accidentally routing traffic to unintended Pods.
  - Example: `selector: { app: nginx, tier: backend }`.

2. **Leverage DNS for Service Discovery**:
  - Prefer DNS-based service discovery (e.g., `nginx-service.default.svc.cluster.local`) over environment variables for flexibility and scalability.
  - Ensure the cluster’s DNS is properly configured (e.g., CoreDNS or kube-dns).

3. **Choose the Right Service Type**:
  - Use **ClusterIP** for internal communication to minimize exposure and conserve resources.
  - Use **NodePort** sparingly, mainly for development or environments without a cloud load balancer.
  - Use **LoadBalancer** for production-grade external access in cloud environments.
  - Use **ExternalName** for seamless integration with external services without proxy overhead.

4. **Configure Health Checks**:
  - Ensure Pods behind a Service have proper liveness and readiness probes to prevent traffic from being routed to unhealthy Pods.
  - Example: Use HTTP probes for web servers to check `/healthz` endpoints.

5. **Optimize Port Configuration**:
  - Use descriptive port names (e.g., `http`, `https`) in Service definitions for clarity.
  - Ensure `targetPort` matches the container’s port, and use `port` for the Service’s exposed port.
  - Example:
    ```yaml
    ports:
    - name: http
      port: 80
      targetPort: 8080
      protocol: TCP
    ```

6. **Use Headless Services When Needed**:
  - For stateful applications (e.g., databases), consider a headless Service (`clusterIP: None`) to directly access individual Pod IPs via DNS.
  - Example: Useful for StatefulSets like MongoDB or MySQL clusters.

7. **Monitor and Log Service Traffic**:
  - Use tools like Prometheus and Grafana (covered in `18-monitoring-logging/`) to monitor Service performance and traffic.
  - Enable logging to debug connectivity issues.

8. **Secure Services**:
  - Restrict Service access using NetworkPolicies (covered in `14-network-policies/`) to limit traffic to specific namespaces or Pods.
  - Use RBAC (covered in `13-rbac/`) to control who can create or modify Services.
  - For LoadBalancer Services, configure cloud provider security groups to restrict access.

9. **Test and Validate Connectivity**:
  - Regularly test Service connectivity using tools like `curl`, `wget`, or `kubectl exec` to ensure proper routing.
  - Verify endpoints with `kubectl get endpoints <service-name>` to confirm Pods are correctly associated.

10. **Document Service Usage**:
  - Include documentation in your manifests (e.g., annotations or comments) to clarify the purpose of each Service.
  - Example: `annotations: { description: "Exposes the frontend API internally" }`.

### When to Use Each Service Type
| **Service Type** | **When to Use** | **Example Scenario** |
|-------------------|-----------------|----------------------|
| **ClusterIP**    | Internal communication between microservices or components within the cluster. | Frontend Pods accessing a backend API within the same cluster. |
| **NodePort**     | Development, testing, or on-premises setups where external access is needed without a cloud load balancer. | Accessing a web app on a Minikube cluster for testing. |
| **LoadBalancer** | Production environments requiring reliable external access in a cloud provider. | Public-facing web application or API in AWS/GCP/Azure. |
| **ExternalName** | Integrating with external services (e.g., SaaS APIs, external databases) without proxying traffic. | Connecting to an external payment gateway like `api.stripe.com`. |

## Exercises
This directory contains hands-on exercises to learn about Kubernetes Services:

1. **01-clusterip-service**: Expose a Pod internally using a ClusterIP Service.
2. **02-nodeport-service**: Expose a Pod externally using a NodePort Service.
3. **03-loadbalancer-service**: Expose a Pod using a LoadBalancer Service.
4. **04-service-discovery**: Explore Service discovery via environment variables and DNS.
5. **05-troubleshooting**: Diagnose and fix issues with a misconfigured Service.

## Prerequisites
- A Kubernetes cluster (e.g., Minikube, Kind, or Docker Desktop) set up from `00-setup/`.
- `kubectl` installed and configured.
- Familiarity with Pods from `02-pods/`.

## Getting Started
Navigate to each exercise directory and follow the instructions in `exercise.md`. Use the provided `Makefile` to apply or delete resources.

Example:
```bash
cd 01-clusterip-service
make apply
kubectl get svc
make delete
```
# Exercise: Configuring Pod Health Checks

## Objective
Learn how to configure liveness and readiness probes for Pods.

## Steps
1. Apply the Pod manifest using `make apply`.
2. Check the Pod status with `kubectl get pods`.
3. Describe the Pod to see probe details: `kubectl describe pod health-check-pod`.
4. Simulate a failure by modifying the liveness probe path to an invalid one and observe Kubernetes restarting the container.
5. Delete the Pod using `make delete`.

## Pod Definition
The `pod.yml` file defines a Pod with liveness and readiness probes for an NGINX container.
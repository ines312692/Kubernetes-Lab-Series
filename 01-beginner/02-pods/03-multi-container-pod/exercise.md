# Exercise: Multi-Container Pod

## Objective
Learn how to run multiple containers in a single Pod and understand their interactions.

## Steps
1. Apply the Pod manifest using `make apply`.
2. Check the Pod status with `kubectl get pods`.
3. View logs for each container: `kubectl logs multi-container-pod -c nginx` and `kubectl logs multi-container-pod -c sidecar`.
4. Exec into the Pod to explore shared resources: `kubectl exec -it multi-container-pod -c nginx -- /bin/bash`.
5. Delete the Pod using `make delete`.

## Pod Definition
The `pod.yml` file defines a Pod with two containers: an NGINX server and a sidecar container running a simple script.
# Exercise: Creating a Basic Pod

## Objective
Learn how to create and manage a simple Pod in Kubernetes.

## Steps
1. Apply the Pod manifest using `make apply`.
2. Check the Pod status with `kubectl get pods`.
3. View Pod details with `kubectl describe pod nginx-pod`.
4. Access the Pod's logs with `kubectl logs nginx-pod`.
5. Delete the Pod using `make delete`.

## Pod Definition
The `pod.yml` file defines a simple Pod running an NGINX container.

## Commands to Try
- `kubectl get pods -o wide`
- `kubectl exec -it nginx-pod -- /bin/bash`
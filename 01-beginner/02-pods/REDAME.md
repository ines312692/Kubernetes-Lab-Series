# 02-pods

This directory contains hands-on exercises to learn about Kubernetes Pods, the basic deployable units in Kubernetes.

## Exercises
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
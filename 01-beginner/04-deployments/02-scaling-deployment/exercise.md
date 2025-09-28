# Exercise: Scaling a Deployment

## Objective
Learn how to manually scale a Deployment to adjust the number of replicas.

## Steps
1. Apply the Deployment manifest using `make apply`.
2. Verify the initial replicas with `kubectl get deployments`.
3. Scale the Deployment to 5 replicas: `kubectl scale deployment nginx-deployment --replicas=5`.
4. Check the new Pods: `kubectl get pods -l app=nginx`.
5. Scale back to 2 replicas and verify.
6. Delete the Deployment using `make delete`.

## Deployment Definition
The `deployment.yml` file defines a Deployment with an initial 3 replicas.

## Commands to Try
- `kubectl get replicasets`
- `kubectl edit deployment nginx-deployment` (to scale declaratively)
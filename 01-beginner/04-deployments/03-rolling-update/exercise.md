# Exercise: Performing a Rolling Update

## Objective
Learn how to perform a rolling update to update a Deployment’s container image.

## Steps
1. Apply the Deployment manifest using `make apply`.
2. Verify the Deployment with `kubectl get deployments`.
3. Update the image to `nginx:1.21`: `kubectl set image deployment/nginx-deployment nginx=nginx:1.21`.
4. Monitor the rollout status: `kubectl rollout status deployment/nginx-deployment`.
5. Check the new Pods: `kubectl get pods -l app=nginx`.
6. Delete the Deployment using `make delete`.

## Deployment Definition
The `deployment.yml` file defines a Deployment with a rolling update strategy.

## Commands to Try
- `kubectl describe deployment nginx-deployment`
- `kubectl get replicasets`
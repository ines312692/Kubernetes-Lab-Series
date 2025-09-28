# Exercise: Rolling Back a Deployment

## Objective
Learn how to roll back a Deployment to a previous version.

## Steps
1. Apply the Deployment manifest using `make apply`.
2. Update the image to `nginx:1.21`: `kubectl set image deployment/nginx-deployment nginx=nginx:1.21`.
3. Check the rollout history: `kubectl rollout history deployment/nginx-deployment`.
4. Roll back to the previous version: `kubectl rollout undo deployment/nginx-deployment`.
5. Verify the rollback: `kubectl rollout status deployment/nginx-deployment`.
6. Delete the Deployment using `make delete`.

## Deployment Definition
The `deployment.yml` file defines a Deployment with revision history enabled.

## Commands to Try
- `kubectl describe deployment nginx-deployment`
- `kubectl rollout history deployment/nginx-deployment --revision=1`
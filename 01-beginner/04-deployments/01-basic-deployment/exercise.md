
### 01-basic-deployment/
**exercise.md**
```markdown
# Exercise: Creating a Basic Deployment

## Objective
Learn how to create and manage a Deployment with multiple Pod replicas.

## Steps
1. Apply the Deployment manifest using `make apply`.
2. Verify the Deployment with `kubectl get deployments`.
3. Check the Pods created by the Deployment: `kubectl get pods -l app=nginx`.
4. Describe the Deployment to see details: `kubectl describe deployment nginx-deployment`.
5. Delete the Deployment using `make delete`.

## Deployment Definition
The `deployment.yml` file defines a Deployment with three replicas of an NGINX container.

## Commands to Try
- `kubectl get replicasets`
- `kubectl describe pod <pod-name>`
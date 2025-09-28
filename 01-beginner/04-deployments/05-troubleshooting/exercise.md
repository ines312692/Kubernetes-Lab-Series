# Exercise: Troubleshooting Deployments

## Objective
Learn how to diagnose and fix issues with a misconfigured Deployment.

## Steps
1. Apply the broken Deployment manifest using `make apply`.
2. Check the Deployment status: `kubectl get deployments`.
3. Describe the Deployment to identify issues: `kubectl describe deployment broken-deployment`.
4. Check Pod status and logs: `kubectl get pods` and `kubectl logs <pod-name>`.
5. Fix the issue in `deployment-broken.yml` (hint: check the image name) and reapply.
6. Delete the Deployment using `make delete`.

## Deployment Definition
The `deployment-broken.yml` file contains an intentional error (invalid image name).

## Commands to Try
- `kubectl get replicasets`
- `kubectl describe pod <pod-name>`
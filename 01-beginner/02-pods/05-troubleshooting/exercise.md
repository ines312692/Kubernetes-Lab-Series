# Exercise: Troubleshooting Pods

## Objective
Learn how to diagnose and fix issues with a misconfigured Pod.

## Steps
1. Apply the broken Pod manifest using `make apply`.
2. Check the Pod status with `kubectl get pods` and note any errors.
3. Describe the Pod to identify issues: `kubectl describe pod broken-pod`.
4. View logs for clues: `kubectl logs broken-pod`.
5. Fix the issue in `pod-broken.yml` (hint: check the image name) and reapply.
6. Delete the Pod using `make delete`.

## Pod Definition
The `pod-broken.yml` file contains an intentional error (invalid image name) to practice troubleshooting.
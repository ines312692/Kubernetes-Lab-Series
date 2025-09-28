# Exercise: Troubleshooting Services

## Objective
Learn how to diagnose and fix issues with a misconfigured Service.

## Steps
1. Apply the Pod and broken Service manifests using `make apply`.
2. Check the Service status with `kubectl get svc`.
3. Describe the Service to identify issues: `kubectl describe svc broken-service`.
4. Test connectivity: `kubectl run test-pod --image=busybox --restart=Never -- wget -q -O - http://broken-service`.
5. Fix the issue in `service-broken.yml` (hint: check the selector) and reapply.
6. Delete the resources using `make delete`.

## Definitions
- `pod.yml`: Defines an NGINX Pod.
- `service-broken.yml`: Contains an intentional error (mismatched selector labels).
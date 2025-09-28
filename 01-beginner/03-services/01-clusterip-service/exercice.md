# Exercise: Creating a ClusterIP Service

## Objective
Learn how to expose a Pod internally using a ClusterIP Service.

## Steps
1. Apply the Pod and Service manifests using `make apply`.
2. Verify the Service creation with `kubectl get svc`.
3. Access the Service from another Pod: `kubectl exec -it test-pod -- curl http://nginx-service`.
4. Check Service endpoints with `kubectl get endpoints nginx-service`.
5. Delete the resources using `make delete`.

## Definitions
- `pod.yml`: Defines an NGINX Pod.
- `service.yml`: Defines a ClusterIP Service to expose the Pod internally.

## Commands to Try
- `kubectl describe svc nginx-service`
- `kubectl get pods -l app=nginx`
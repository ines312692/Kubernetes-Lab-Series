# Exercise: Creating a LoadBalancer Service

## Objective
Learn how to expose a Pod using a LoadBalancer Service (cloud or Minikube with add-ons).

## Steps
1. Apply the Pod and Service manifests using `make apply`.
2. Verify the Service creation with `kubectl get svc`.
3. Get the external IP with `kubectl get svc nginx-loadbalancer`.
4. Access the Service via the external IP (e.g., `curl http://<external-ip>`).
5. Delete the resources using `make delete`.

## Definitions
- `pod.yml`: Defines an NGINX Pod.
- `service.yml`: Defines a LoadBalancer Service.

## Note
LoadBalancer Services require a cloud provider or Minikube with the `tunnel` command (`minikube tunnel`).
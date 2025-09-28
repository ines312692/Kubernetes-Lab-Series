# Exercise: Creating a NodePort Service

## Objective
Learn how to expose a Pod externally using a NodePort Service.

## Steps
1. Apply the Pod and Service manifests using `make apply`.
2. Verify the Service creation with `kubectl get svc`.
3. Access the Service externally using the NodePort (e.g., `curl http://<node-ip>:<node-port>`).
4. Check the allocated NodePort with `kubectl describe svc nginx-nodeport`.
5. Delete the resources using `make delete`.

## Definitions
- `pod.yml`: Defines an NGINX Pod.
- `service.yml`: Defines a NodePort Service to expose the Pod externally.

## Note
Ensure your cluster nodes are accessible (e.g., Minikube: `minikube ip`).
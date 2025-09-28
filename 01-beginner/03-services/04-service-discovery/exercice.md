# Exercise: Service Discovery

## Objective
Learn how Kubernetes Services enable discovery via environment variables and DNS.

## Steps
1. Apply the Pod and Service manifests using `make apply`.
2. Create a test Pod: `kubectl run test-pod --image=busybox --restart=Never -- /bin/sh -c 'sleep 3600'`.
3. Exec into the test Pod: `kubectl exec -it test-pod -- /bin/sh`.
4. Check environment variables: `env | grep NGINX_SERVICE`.
5. Test DNS resolution: `nslookup nginx-service`.
6. Access the Service: `wget -q -O - http://nginx-service`.
7. Delete the resources using `make delete`.

## Definitions
- `pod.yml`: Defines an NGINX Pod.
- `service.yml`: Defines a ClusterIP Service.
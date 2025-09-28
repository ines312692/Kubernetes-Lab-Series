# Exercise: Managing Pods with Labels

## Objective
Understand how to use labels to organize and select Pods.

## Steps
1. Apply the Pod manifest using `make apply`.
2. List Pods with specific labels using `kubectl get pods -l app=frontend`.
3. Update the Pod's labels with `kubectl label pod nginx-pod environment=production`.
4. Verify the label update with `kubectl get pods --show-labels`.
5. Delete the Pod using `make delete`.

## Pod Definition
The `pod.yml` file defines a Pod with labels for application and environment.
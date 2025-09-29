.PHONY: apply delete

apply:
	kubectl apply -f configmap.yml
	kubectl apply -f pod.yml

delete:
	kubectl delete -f pod.yml
	kubectl delete -f configmap.yml
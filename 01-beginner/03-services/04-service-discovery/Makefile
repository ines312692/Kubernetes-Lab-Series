.PHONY: apply delete

apply:
	kubectl apply -f pod.yml
	kubectl apply -f service.yml

delete:
	kubectl delete -f service.yml
	kubectl delete -f pod.yml
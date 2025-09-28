.PHONY: apply delete

apply:
	kubectl apply -f pod.yml
	kubectl apply -f service-broken.yml

delete:
	kubectl delete -f service-broken.yml
	kubectl delete -f pod.yml
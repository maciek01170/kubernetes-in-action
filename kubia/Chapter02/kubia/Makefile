IMAGE ?= kubia:latest

-include ../../Makefile.overrides
-include ../Makefile.overrides
-include Makefile.overrides

run:
	make image push
	kubectl delete po kubia --force --grace-period 0
	kubectl apply -f ../pod.kubia.yaml && kubectl wait --for=condition=Ready pod/kubia && kubectl port-forward kubia 8080

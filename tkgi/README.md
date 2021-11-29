````bash
kubeclt apply -f kubia-manual-with-labels.yaml
kubectl apply -f kubia-rc.yaml
kubectl apply -f kubia-svc.yaml
Error from server (Forbidden): error when creating "kubia-svc.yaml": services "kubia" is forbidden: exceeded quota: reqo-svc0080vskt0001-maciej, requested: services.nodeports=1, used: services.nodeports=0, limited: services.nodeports=0
````

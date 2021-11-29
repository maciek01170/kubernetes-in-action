# Kubernetes in Action, 2nd Edition

## Chapter 7. Mounting storage volumes into the Pod's containers

### Fortune pod 
#### without volume: pod.quiz.novolume.yaml
No volume, must be populated at each restart
```
# launch
kubectl apply - f pod.quiz.novolume.yaml
# number of entrie in the DB: 0
kubectl exec -it quiz -c mongo -- mongo kiada --quiet --eval "db.questions.count()"
# insert questions 
./insert-questions.sh
# now the number of entries must be 1
# test all the stuff
kubectl port-forward quiz 8080
curl localhost:8080/questions/random
```
#### with empty volume: pod.quiz.emptydir.yaml
Volume created and destroyed with the pod, must be populated at each restart

#### with empty volume + init in container: pod.quiz.emptydir.init.yaml
Two volumes: 
* initdb: javascript code with db init
* quiz-data: mongo data
Three containers:
* init: just copy the JS code to /initdb
* mongo: 
  * mounts the initdb content in /docker-entrypoint-initdb.d/ -> this script is evaluated at container startup
  * uses /quiz-data as DB (populated at init)

#### pod with two containers and a shared volume: pod.quote.yaml
```
# launch
kubectl apply - f pod.quote.yaml
kubectl exec quote -c quote-writer -- cat /var/local/output/quote
kubectl exec quote -c nginx -- cat /usr/share/nginx/html/quote
```

#### Persisting vol with Docker Desktop and macOS
* https://towardsdatascience.com/persistent-volume-for-kubernetes-w-acl-support-on-mac-4bf604f3a65d
* created dir: /Users/maciek/epfl/k8s/data/tmp/vol





### Fortune pod with or without a volume
- [fortune-no-volume.yaml](fortune-no-volume.yaml) - YAML manifest file for the `fortune-no-volume` pod
- [fortune-emptydir.yaml](fortune-emptydir.yaml) - YAML manifest file for the `fortune-emptydir` pod with an emptyDir volume
- [fortune.yaml](fortune.yaml) - YAML manifest file for the `fortune` pod with two containers that share a volume

### MongoDB pod with external volume
- [mongodb-pod-gcepd.yaml](mongodb-pod-gcepd.yaml) - YAML manifest file for the `mongodb` pod using a GCE Persistent Disk volume
- [mongodb-pod-aws.yaml](mongodb-pod-aws.yaml) - YAML manifest file for the `mongodb` pod using an AWS Elastic Block Store volume
- [mongodb-pod-nfs.yaml](mongodb-pod-nfs.yaml) - YAML manifest file for the `mongodb` pod using an NFS volume
- [mongodb-pod-hostpath.yaml](mongodb-pod-hostpath.yaml) - YAML manifest file for the `mongodb` pod using a hostPath volume (for use in _Minikube_)
- [mongodb-pod-hostpath-kind.yaml](mongodb-pod-hostpath-kind.yaml) - YAML manifest file for the `mongodb` pod using a hostPath volume (for use in _kind_)

### Using a hostPath volume
- [node-explorer.yaml](node-explorer.yaml) - YAML manifest file for the `node-explorer` pod
- [node-explorer-directory.yaml](node-explorer-directory.yaml) - YAML manifest file for the `node-explorer` pod with the hostPath volume type set to Directory


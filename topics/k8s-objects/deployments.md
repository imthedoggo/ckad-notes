## Deployments
rolling updates are constant version updates which do not result in downtime for users (updating instances one by one)
(similar to replicaSet)


## 

### YAML
```angular2html
piVersion: apps/v1
kind: Deployment
metadata:
spec:
	template:
	replicas:
	selector:
```

### imperative

```angular2html
kubectl get deploy
kubectl create -f deployment-definition.yml

see pods, replicaSets and Deployments
kubectl get all
```
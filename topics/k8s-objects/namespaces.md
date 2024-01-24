## Namespaces

namespace isolation- contained environment, isolating resources wihin a namespace
e.g stage and prod will have the same cluster with different resources
each namespace can have its own set of policies (who can do what)

kube-system, Default, kube-public

resources in a namespace can refer to each other by their names



create a pod in default namespace
kubectl create -f pod-definition.yml




to create a certain pod by default in a specific namespace without specifying, move namespace attribute in the pod definition  file, under metadata


### YAML

```angular2html
apiVersion: v1
kind: Namespace
metadata:
	name: dev
```

#### ResourceQuote for a namespace
```angular2html
apiVersion: v1
kind: ResourceQuota
metadata:
	name: compute-quota
	namespace: dev
spec:
	hard:
		pods: "10"
		requests.memory: 5Gi
```
kubectl create -f compute-quota.yaml

### imperative

create namespace, 2 options:
`kubectl create -f namespace-dev.yml`
`kubectl create namespace dev`

list pods in a namespace that isn't the default
`kubectl get pods --namespace=kube-system`

view all pods in all namespaces
`kubectl get pods --all-namespaces`

create a pod in a non-default namespace
`kubectl create -f pod-definition.yml --namespace=dev`

switch to dev namespace to work on it instead of default
`kubectl config set-context $(kubectl config current-context) --namespace=dev`

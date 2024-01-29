
## Taints
nodes-pods, what pods can be scheduled on a node

taint - rule applied on a node, that disqualifies pods to run on that node
toleration - a 'whitelisting' of a pod, to tolerate the taint applied on a node
A node can only accept pods that tolerate it.

k taint nodes node-name key=value:TaintEffect
what happens to pods that don't tolerate the taint?
NoSchedule | PreferNoSchedule | NoExecute

There's no guarantee that the tolerating pod will be assigned to the tainted node!

the scheduler doesn't schedule pods on the master node, because a taint is set on the master node automatically - prevents pod scheduling
it's a best practice not to schedule nodes on a master node (only worker nodes)
`k describe node kubemaster | grep Taint`

## Tolerations




```angular2html
pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
	name: myapp-pod
spec:
	containers:
	- name: nginx-container
	  image: nginx

	tolerations:  
	- key: "app"
	  operator: "Equal"
	  value: "blue"
	  effect: "NoSchedule"
```


### imperative

`k describe node kubemaster | grep Taint`
## Node selectors
set limitation on pods to run on particular nodes (to utilise resources better)

`k label nodes node1 size=Large`
then when we create the pod, it assigned to node1 as desired
in case of more sophisticated requirement OR ot NOT, e.g large or medium but not small -> node affinity

```angular2html
pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
	name: myapp-pod
spec:
	containers:
	- name: data-processor
	  image: data-processor

	nodeSelector:
		size: Large
```
large -> lables and selectors


## Node Affinity

Node Affinity Types:

- requiredDuringSchedulingIgnoreDuringExecution
- preferredDuringSchedulingIgnoreDuringExecution
- requiredDuringSchedulingRequiredDuringExecution



          During Scheduling   During Execution
Type 1.    required           Ignored
Type 2     preferred          Ignored
Type 3 (future) Required      Required

```angular2html
pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
	name: myapp-pod
spec:
	containers:
	- name: data-processor
	  image: data-processor

	affinity:
		nodeAffinity:
			requiredDuringSchedulingIgnoreDuringExecution:
				nodeSelectorTerms:
				- matchExpressions:
					- key: size
					  operator: In
					  values:
					  - Large

					- key: size
					  operator: NotIn
					  values:
					  - Small

					- key: size
					  operator: Exists
```


### imperative


lable a node
`k label nodes <node-name> <lable-key>=<lable-value>`
`k label nodes node1 size=Large`

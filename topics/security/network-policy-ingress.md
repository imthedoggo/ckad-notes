## Network policy

Traffic
for a webserver, incoming users traffic is an ingress traffic
an outgoing request to the BE APIs in an Egress traffic
it is relational to the service
1 ingress on 80
2 Ergress on 5000
3 ingress on 5000
4 Ergress on 5432

pods should be able to communicate with each other without additional settings (e.g routes)
However, sometimes we wanna restrict access between specific functions (e.g web and DB pods) so ->
Link a network policy into one or more pods
e.g allow ingress traffic from API pod on port 3306
when the pod is created it blocks all other ingress traffic

we use labels and selectors
specify whether to allow ingress/egress traffic or both

Once you allow ingress traffic, the response to that traffic (egress to the requester) is allowed automatically

multiple pods in cluster with same labels but different namespaces
by default all namespaces are allowed
if we waana restrict traffic for a specific namespace only: NAMESPACE SELECTOR

in case we only have a namespace selector and not a pod selector, all pods in that namespace can reach the pod

if a backup service outside the cluster needs to reach it, namespace/selectors won't work (outside the cluster)
but we know the IP address of that server -> we can whitelist it using ipBlock
network policy is applied on a pod

* inside the block : AND operation (allow traffic where namespace AND podSelector)
* outside the block: OR operation (allow ipBlock OR selector definition)
  if we add a dash, AKA another rule to the array, it will mean an OR operation


### YAML
block all traffic to the DB pod except for traffic from the API pod
```angular2html
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
	name: db-policy
spec: 	
	podSelector:
		matchLabels:
			role: db

	policyTypes:
	- Ingress
	ingress:
	-from:
	  -podSelector:
	  	matchLabels:
	  		name:apiPod
	  ports:
	  - protocol: TCP
	    port: 3306		
```

```angular2html
kind: NetworkPolicy
metadata:
	name: db-policy
spec: 	
	podSelector:
		matchLabels:
			role: db

	policyTypes:
	- Ingress
	ingress:
	-from:
	  - podSelector:
	  	matchLabels:
	  		name:apiPod

	  	namespaceSelector:
	  		matchLabels:
	  			name: prod	

	  ports:
	  - protocol: TCP
	    port: 3306		
```
whitelist service outside the cluster with IP Block
```angular2html
spec: 	
	podSelector:
		matchLabels:
			role: db

	policyTypes:
	- Ingress
	ingress:
	-from:
	  - podSelector:
	  		matchLabels:
	  			name:apiPod
	  ipBlock:
	  	cidr:192.168.5.10/32		

	  ports:
	  - protocol: TCP
	    port: 3306	
```

Egress -> traffic from pod to the backup server
we need an Egress rule defined
```angular2html
spec: 	
	podSelector:
		matchLabels:
			role: db

	policyTypes:
	- Ingress
	- Egress
	ingress:
	-from:
	  - podSelector:
	  		matchLabels:
	  			name:apiPod
	  ipBlock:
	  	cidr:192.168.5.10/32		
	egress:
	- to:   
		- ipBlock:
			cidr: 192.168.5.10/32

	  ports:
	  - protocol: TCP
	    port: 80
```

### imperative

`k get netpol`




`kubectl create ingress ingressname --rule="ckad-mock-exam-solution.com/video*=my-video-service:8080" --dry-run=client -oyaml > ingress.yaml`

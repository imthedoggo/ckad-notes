## Pods

#### Lifecycle:
POD statuses

* pending state (scheduler searched for a node to place the pod) see description as to why
* ContainerCreating: once scheduled, images required for applications are pulled
* Running: once all the containers in the pod start

Conditions compliment pod status
array of boolean values that describes the exact state

- PodScheduled:
- Initialized:
- ContainersReady: different apps can be running, script job, db service, web server... takes different amountof time
- Ready: indicates that the app is running inside the pod
  Ready condition needs to be tied up to real readiness.

#### Readiness probes
Ready condition can be set to true upon a successful probe result (e.g http request sent, is TCP socket listening)
failureThreshold option by default gives 3 attempts, but can be overridden

For a replicaSet / deployment with multiple pods
with the rght readiness probe configuration, additional pods won't be served traffic before ready, so that the old ones can still cover all the traffic

#### Liveness probe
LIVENESS PROBE can be configured to check weather the container is healthy and app is available, and if not trigger a reatart
eg API is running/TCP socket is listening

let's say the nginx process crashes and the container quits due to some error
docker however is not orchestrated and the container still down, until manually creating a new container
```angular2html
docker run nginx
docker ps -a
k run nginx --image=nginx
```

the webapp now starting with k8s

if the app crashes, k8s tries to restart the app (see restarts amount)
in case of a infinite loop due to a bug in the application, the container is up but the app won't serve its users (container to be restarted)

### YAML

```angular2html
apiVersion: v1
lind: Pod
metadata:
	name: webapp
	labels:
		name: webapp
spec:
	containers:
	- name: webapp
		image: webapp
		ports:
			- containerPort: 8080

		livenessProbe:
			httpGet:
				path: /api/healthy
				port: 
			initialDelaySeconds: 10
			periodSeconds: 5
			failureThreshold: 8	
	
		readinessProbe:		
			tcpSocket:
				port: 3306	
			exec:
				command:
					- cat
					- /app/is_healthy	
```


### Imperative

get available pods
`kubectl get pods`

see info on pod
`kubectl describe pod pod-name`

see info on all pods (node, ip, restarts)
`kubectl get pods -o wide`

run with image and save into a yaml
`kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml`

create a pod based on the file specs
`kubectl create -f pod-definition.yml`

modify properties
`kubectl edit pod pod-name`

update resource with changes made to the file, with force
`kubectl replace -f simple-webapp-2.yaml --force`

apply changes of file
`kubectl apply -f redis.yaml`

if not given a pod definition file, extract it to a file:
`kubectl get pod pod-name -o yaml > pod-definition.yaml`

delete
`kubectl delete pod webapp`

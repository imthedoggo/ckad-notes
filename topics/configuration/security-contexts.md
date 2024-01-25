## Security Contexts
containers are encapsulated in pods
One can choose to configure security settings on a pod or container level

Defining on a pod level will carry over the settings to all the containers within the pod
if both -> container settings will override the pod ones.
capabilities can be added separately


### YAML
Pod level:
```angular2html
apiVersion: v1
kind: Pod
metadata:
	name: web-pod
spec:
	securityContext:
		runAsUser: 1000
	containers:
		- name: ubuntu
		image: ubuntu
		command: ["sleep", "3600"]
```

On container level:
```angular2html
apiVersion: v1
kind: Pod
metadata:
	name: web-pod
spec:
	containers:
		- name: ubuntu
		image: ubuntu
		command: ["sleep", "3600"]
	securityContext:
		runAsUser: 1000
		capabilities:
			add: ["MAC_ADMIN"]	
```

### important attributes
* securityContext.runAsUser:
* securityContext.capabilities
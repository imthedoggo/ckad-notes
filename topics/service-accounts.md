user vs. service account

service account token is created automatically, will be used by the application-------------
from 1.24 need to create manually with:
kubectl create token service-acount-name
token will be printed, expiration date is defined for an hour
--------------
stored as a secret object, then linked to the service account token attribute

each namespace has its own default serviceAccount

```angular2html
k get sa
k describe serviceaccount dashboard-sa
k describe secret dashboard-token-kbdm
```

k8s automatically mounts the default service account if you haven't explicitly specified any

### YAML
```angular2html
apiVersion: v1
kind: Pod
metadata:
	name: k8s-dashboard
spec:
	securityContext:
		runAsUser: 1000
	containers:
		- name: k8s-dashboard
		image: k8s-dashboard
	serviceAccountName: dashboard-sa
```


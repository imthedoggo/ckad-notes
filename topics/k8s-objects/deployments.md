## Deployments
Deployment is an object which can own ReplicaSets and update them and their Pods via declarative, server-side rolling updates.
rolling updates are constant version updates which do not result in downtime for users (updating instances one by one)

## 

### YAML
```angular2html
apiVersion: apps/v1
kind: Deployment
metadata:
spec:
	template:
	replicas:
	selector:
```

Once you create a PVC use it in a deployment definition file by specifying the PVC Claim name under persistentVolumeClaim section in the volumes section like this:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeploy
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

### imperative

```angular2html
kubectl get deploy
kubectl create -f deployment-definition.yml

see pods, replicaSets and Deployments
kubectl get all
```

print content of webapp container
`kubectl exec webapp -- cat /log/app.log`

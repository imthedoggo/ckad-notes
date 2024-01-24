## Replication controller
the replication controller makes sure the specified amount of pods is running
pod fails -> user has no access to app -> run multiple instances of a single pod for high availability
it can also run on multiple nodes to help scaling the application

Replication Controller ~= Replica Set(the new recommended way)

rc-definition.yaml
```angular2html
apiVersion: v1
kind: ReplicationController
metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: fe
spec:
    template: (metadata + spec of the desired pod)
        metadata:
            app: myapp
            type: front-end
spec: (unique for each created object, dictionary)
    containers:
    - name: nginx-container
        image: nginx
    replicas: 3
```

## Replica Set
can also manage pods that weren't created as a part of the replica set

### YAML
```angular2html

apiVersion: apps/v1
kind: ReplicaSet
metadata:
	name:
	labels:
spec:
	template:
		metadata: 
			app: myapp
			type: front-end
		spec: (unique for each created object, dictionary)
			containers: 
				- name: nginx-container
				image: nginx
	replicas: 3
	selector: (not required, assumed as lables provided)
		matchLabels:
			type: front-end

```

## important 
* replicas
* selector.matchLabels

## Labels and Selectors

ensuring 3 running pods
how to monitor which pods? with labeles!

selector should match template labels

### imperative

```angular2html
kubectl get rs
kubectl create -f replicaset-definition.yml
kubectl replace -f replicaset-defininition.yaml
kubectl delete replicaset myapp-repicaset
```

scale
```angular2html
kubectl scale --replicas=6 -f replicaset-definition.yml
kubectl scale --replicas=6 replicaset myapp-replicaset
```
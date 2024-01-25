## What problem does it solve

data is destroyed together with the deletion of a container, volumes are a way to persist data -> data remains.

A volume needs a storage.
volumes is an attribute, together with containers (under spec)

### set-up
needs to do 2 things:
* specify a directory on the host machine to store the data (volumes, on containers level)
* specify a directory on the container that will be bound to the storage directory (volumeMounts, inside container)

## Persistent Volumes
in a large env volumes are configures on all pod definitions (otherwise each change should be made everywhere, on each pod definition).
instead, configure it globally ->
CLUSTER-wide pool of storage volumes, configured by an administrator, to be used by users (select storage from this pool)

**spec**
* accessModes
  * ReadWriteOnce (RWO): the volume can be mounted as read-write by a single node (still can allow multiple pods to access the volume when the pods are running on the same node)
  * ReadOnlyMany (ROX): the volume can be mounted as read-only by many nodes
  * ReadWriteMany (RWX): the volume can be mounted as read-write by many nodes
  * ReadWriteOncePod (RWOP): the volume can be mounted as read-write by a single Pod
* capacity:
* hostPath: 

## Persistent Volume Claims
User creates claims to use the storage.
PVCs can remain in a pending state if no volumes are available
It binds pv-pvc based on attributes:
* accessMode
* resources.requests.storage
* volumeName

Pods access storage by using the claim as a volume.
Claims must exist in the same namespace as the Pod using the claim. The cluster finds the claim in the Pod's namespace and uses it to get the PersistentVolume backing the claim. The volume is then mounted to the host and into the Pod.

when a claim is deleted, we can choose what will happen to the PV
by default set to Retain (remain until manually deleted). Alternatively, it can be deleted automatically or recycled
* Retain
* Delete
* Recycle

### YAML
Volume on a pod-container
```angular2html
apiVersion: v1
kind: Pod
metadata:
	name:
		number-generator
spec:
	containers:
	- image: alpine
	  name: alpine
	  command: ["/bin/sh", "-c"]
	  args: ["shuf -i 0-100 >> /opt/number.out;]
	  volumeMounts:
	  - mountPath: /opt
	    name: data-volume
	volumes:
	- name: data-volume
	  hostPath:
	  	path: /data
	  	type: Directory
```

with a storage solution, e.g AWS
```angular2html
apiVersion: v1
kind: Pod
metadata:
	name:
		number-generator
spec:
	containers:
	- image: alpine
	  name: alpine
	  command: ["/bin/sh", "-c"]
	  args: ["shuf -i 0-100 >> /opt/number.out;]
	  volumeMounts:
	  - mountPath: /opt
	    name: data-volume
	volumes:
	- name: data-volume
	  awsElasticBlockStore:
	  	volumeId: <id>
	  	fsType: ext4
```
PV
```angular2html
apiVersion: v1
kind: PersistentVolume
metadata:
	name:
		pv-voll
spec:
	accessModes:
		-ReadWriteOnce
	capacity:
		storage: 1Gi
	hostPath:
		path: /tmp/dat
----OR
	awsElasticBlockStore:
	  	volumeId: <id>
	  	fsType: ext4	
```

PVC
```angular2html
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
	name: myclaim
	namespace: develop

spec:
	accessModes:
		-ReadWriteOnce
	resources:
		requests
			storage: 500Mi
	volumeName: pv-voll	
```

Claims As Volumes
```angular2html
apiVersion: v1
kind: Pod
metadata:
  name: mypod
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
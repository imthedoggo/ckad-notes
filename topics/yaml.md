YAML - input for object creation
k8s definition file always contains 4 top level attributes:
apiVersion: v1 (version of k8s api, string)
kind: Pod / replicaSet/  (type of object we are creating, string)
metadata: data about the object, name, lables (dictionary)

```angular2html
name: myapp-pod
labels: (good for filtering and identification, in case of hundreds of pods. Any kind of key-pair values can fit here)
app: myapp
type: front-end
spec: (unique for each created object, dictionary)
containers: (list/array)
- name: nginx-container (a dash means first item in the list)
image: nginx
```

## Tricks

Use List object to create similar objects in the same file
```angular2html
apiVersion: v1
kind: List
items:
  - kind: PersistentVolume
    apiVersion: v1
    metadata:
      name: redis01
    spec:
      accessModes: ["ReadWriteOnce"]
      capacity:
        storage: 1Gi
      hostPath:
        path: /redis01
  - kind: PersistentVolume
    apiVersion: v1
    metadata:
      name: redis02
    spec:
      accessModes: ["ReadWriteOnce"]
      capacity:
        storage: 1Gi
      hostPath:
        path: /redis02
```
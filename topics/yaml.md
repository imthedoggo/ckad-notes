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

## Service



### imperative

Expose commands create a service on the fly, and expose a pod/deployment 

Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
(This will automatically use the pod's labels as selectors)
`k expose pod redis --port=6379 --name redis-service`
Or
(This will not use the pods' labels as selectors; instead it will assume selectors as app=redis)
`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`

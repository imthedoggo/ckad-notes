
## print

print content of webapp container
`kubectl exec webapp -- cat /log/app.log`


## get

switch to dev namespace to work on it instead of default
`kubectl config set-context $(kubectl config current-context) --namespace=dev`

--dry-run: By default, as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource. Instead, tell you whether the resource can be created and if your command is right.

-o yaml: This will output the resource definition in YAML format on the screen



all objects, all namespaces
`k get all -A`

## visibility
check rollout status
`kubectl rollout status deployment/nginx-deployment`

## create

Create an NGINX Pod
`kubectl run nginx --image=nginx`

Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
`kubectl run nginx --image=nginx --dry-run=client -o yaml`

Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
`kubectl run nginx --image=nginx --dry-run=client -o yaml`

save the YAML definition to a file and modify
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yaml

## deploy with replicas

`kubectl create deployment nginx --image=nginx --replicas=4`

Create a new deployment called redis-deploy in the dev-ns namespace with the redis image. It should have 2 replicas
`kubectl create deployment redis-deploy --image=redis --replicas=2 -n dev-ns`

update deployment
`kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1`

## scale

kubectl scale deployment nginx --replicas=4


## 

## expose

Create a new pod called custom-nginx using the nginx image and expose it on container port 8080
`k run custom-nginx --image=nginx --port=8080`

Create a pod:httpd using the image httpd:alpine in the default namespace.
Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.
`k run  httpd --port=80 --image=httpd:alpine --expose`

create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
`k expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`
(This will automatically use the pod's labels as selectors)
Or
`k create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml (This will not use the pods' labels as selectors; instead it will assume selectors as app=redis`

expose an existing deployment as nodeport service (nodeport itself is added on the yaml)
`k expose deploy deploy-name --name service-name --type NodePort --port 80`

### misc
JsonPath is a query language for JSON that allows you to extract specific data from a JSON structure.
The `-o=jsonpath` flag in kubectl is used to format the output of a command using a JsonPath expression.
The expression is used to traverse the JSON structure and extract the desired data.

extract the names of all pods in a specific namespace
`kubectl get pods -n your-namespace -o=jsonpath='{.items[*].metadata.name}'`

extract all namespaces into a file
`k get ns -o=jsonpath='{.items[*].metadata.name}' > /desired/folder/filename.txt`


inspect logs 
`kubectl -n elastic-stack logs kibana`



execute into the container and inspect stored logs
`kubectl -n elastic-stack exec -it app -- cat /log/app.log`

get all resources in all namespaces
`kubectl get all -A`


move file
`mv target-file-in-origin-directory target-directory`

print OS content
`cat /etc/*release*`

enter a specific pod / container and open a pod/container terminal
`kubectl exec -it podname -c containername bash`

print file inside the pod
`kubectl exec -it podname -- cat filepath/name`

### objects created imperatively
* pod


* ns



### kubectl replace vs. apply

#### Handling Existing Resources:

kubectl replace: This command replaces the existing resource with a new one. If the resource doesn't exist, it will create it.
kubectl apply: This command is generally used for creating new resources or updating existing ones. It will create a resource if it doesn't exist, and if it does exist, it will update the resource with the new configuration. It's more flexible in terms of its use for both creation and updates.

#### Conflict Resolution:

kubectl replace: It attempts to replace the resource, and if there's a conflict (e.g., someone else updated the resource since you retrieved it), it will fail. The --force flag can be used to override conflicts.
kubectl apply: It is designed to handle conflicts more gracefully. It uses a "last-applied" annotation on the resource to ensure that changes are applied in the correct order and can handle updates from multiple sources more effectively.

#### Usage Scenarios:

kubectl replace: It's often used when you want to ensure that the resource is completely replaced with a new definition, discarding any changes made to it since it was initially created or retrieved.
kubectl apply: It's commonly used for day-to-day resource management. You can use it to create resources initially and then later use the same command to update those resources with changes.
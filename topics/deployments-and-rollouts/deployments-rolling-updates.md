
a deployment triggers a rollout which creates a new revision
tracking changes, rolling back to previous version of deployment

Strategies:

Recreate
-----------
5 replicas ->
destroy all of them-> then deploy new 5 -> downtime

Default:
Rolling update, one by one, seamless upgrade


how?
_ can update the image revision then k apply -f deploymnet.yaml
OR
k set image deployment/myapp-deployment \ nginx-container=nginx:1.9.1
this however doesn't change the file itself! only the running version

History:
* with recreate, you scale down the replicas to 0 then back to 5
* with RU, scaled down+ up one at a time

#### Rollback 

undo a change
k rollout undo deployment/myapp-deployment

with get replicasets we will see that deployments are back to previous state

check revision status
master $ kubectl rollout history deployment nginx --revision=1

You would have noticed that the "change-cause" field is empty in the rollout history output. We can use the --record flag to save the command used to create/update a deployment against the revision number.

master $ kubectl set image deployment nginx nginx=nginx:1.17 --record

To rollback to specific revision we will use the --to-revision flag.
With --to-revision=1, it will be rolled back with the first image we used to create a deployment as we can see in the rollout history output.

`controlplane $ kubectl describe deployments. nginx | grep -i image:`
Image: nginx:1.16

Set image
k set image deployment/myapp-deployment nginx-container=nginx:1.12-perl


Weird number of replicas/desired.up-to-date and a 'stuck' process of a rollout status indicate some error on the backend- likely the deploymnt file

### imperative

see rollout status
`k rollout status deployment/myapp-deployment`

show revisions and history
`k rollout history deployment/myapp-deployment`


Rolling update it's a default -> no need to specify it. Just change the version and 
`k apply -f deploy-update.yaml` or edit

### Rollback

`k rollout undo deployment/nginxdeployment`


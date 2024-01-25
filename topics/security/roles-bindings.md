

## Cluster Roles

Cluster-wide, not a part of a namespace

specify info such as:
* allowed resource types
* allowed specific resources
* allowed actions (verbs) on resources

## Cluster role-bindings 

### imperative

`k describe clusterrole system:controller:cronjob-controller`

count amount of roles, no headers
`kubectl get clusterroles --no-headers  | wc -l`
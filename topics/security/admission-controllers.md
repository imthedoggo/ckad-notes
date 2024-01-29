## Admission controllers

* help implementing security measures
* validate configuration
* perform operations before the pod gets created


`k get pods -n kube-system`

enabled by default: 
* NamespaceLifecycle
* Mutation 


Not by default: 
* NamespaceAutoProvision
## API versions

In Kubernetes versions : X.Y.Z
X stands for major, Y stands for minor and Z stands for patch version.


A resource is a part of an API group
check `k explain resource`

To identify the preferred version, run the following commands as follows :-

root@controlplane:~# kubectl proxy 8001&
root@controlplane:~# curl localhost:8001/apis/authorization.k8s.io
Where & runs the command in the background and kubectl proxy command starts the proxy to the kubernetes API server.

to enable the v1alpha1 version for rbac.authorization.k8s.io API group on the controlplane node.
Add the --runtime-config=rbac.authorization.k8s.io/v1alpha1 option to the kube-apiserver.yaml file.

`cd /etc/kubernetes/manifests/`
`vi kube-apiserver.yaml`



As a good practice, take a backup of that apiserver manifest file before going to make any changes.
root@controlplane:~# cp -v /etc/kubernetes/manifests/kube-apiserver.yaml /root/kube-apiserver.yaml.backup

open up the kube-apiserver manifest file
root@controlplane:~# kubectl get po -n kube-system

```angular2html
{
  "kind": "APIGroup",
  "apiVersion": "v1",
  "name": "authorization.k8s.io",
  "versions": [
    {
      "groupVersion": "authorization.k8s.io/v1",
      "version": "v1"
    }
  ],
  "preferredVersion": {
    "groupVersion": "authorization.k8s.io/v1",
    "version": "v1"
  }
}
```
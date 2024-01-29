## Security primitives
Secure hosts
* password based auth
* ssh key based auth

secure k8s
--------------
kube-api server is in the center of all ops
- who can access the cluster?
- what can they do?


TLS Certificates

Authentication
users accessing the cluster:
- admins
- devs
- app end users
- bots

Admin purposes
- User
- Service Accounts
  k8s can only manage service accounts (not user account)

accessing the API through kubectl or directly via curl, they go through kube-cpi server
authenticates request before processing

static password and token files

## Kube Config

if sotred under $HOME/.kube/config -> don't need to specify the path in the kubectl

the config file has 3 sections:
- clusters: multiple clusters for envs /organsations

- contexts: defines which user account can access to which cluster. connects the cluster-user

- users: user accounts (admin user, app user...)

```angular2html
apiVersion: v1
kind: Config

current-context: dev-user@google

clusters:
- name: my-kuce-playground
  cluster:
  	certificate-authority: ca.crt OR /etc/kubernetes/pki/ca.crt
  	OR
  	certificate-authority-data: sdjfhsd,kfhdsfj (content converted to base64)

  	server: https://my-kube-playground:6443

contexts:
- name: my-kube-admin@my-kube-playground
  context:
  	cluster: my-kube-playground
  	user: my-kube-admin
  	namespace: finance

users:
- name: my-kube-admin
  user:
  	client-certificate: admin.crt
  	client-key: admin.key
```


how does kubectl know which context to choose from?
default context (current-context)

see configs, contexts
kubectl config view

also
kubectl config view --kubeconfig=my-custom-config

update current context
kubectl config use-context prod-user@production

additional commands
kubectl config -h

namespaces
configure a context to switch to a particular namespace


certificates
convert to base64
cat ca.crt | base64

decode

echo "fgdfsgfdg" | base64 --decode
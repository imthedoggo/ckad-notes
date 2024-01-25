


### YAML
```angular2html
apiVersion: v1
kind: Secret
metadata:
  name: bootstrap-token-5emitj
  namespace: kube-system
type: bootstrap.kubernetes.io/token
data:
  auth-extra-groups: c3lzdGVtOmJvb3RzdHJhcHBlcnM6a3ViZWFkbTpkZWZhdWx0LW5vZGUtdG9rZW4=
  expiration: MjAyMC0wOS0xM1QwNDozOToxMFo=
  token-id: NWVtaXRq
```



kubectl get secrets
kubectl describe secrets
kubectl get secret app-secret -o yaml
kubectl create -f secrets.yaml

### imperative

kubectl create secret generic
`<secret-name> --from-literal=<key>=<value>`

`kubectl create secret generic \
app-secret
--from-literal=DB_Host= mysql \
--from-literal=DB_User=root`

`kubectl create secret
<secret-name> --from-file=<path-to-file>
app-secret --from-file=app_secret.properties`

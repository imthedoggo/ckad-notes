


### YAML
```angular2html
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
```


declarative:
kubectl create -f config-map.yaml


kubectl get configmaps
kubectl describe configmaps




### imperative
kubectl create configmap <config-name> --from-literal=<key>=<value>

kubectl create configmap \
app-config --from-literal=APP_COLOR= blue \
-- from-literal=APP_MOD=prod

kubectl create configmap
<config-name> --from-file=<path-to-file>


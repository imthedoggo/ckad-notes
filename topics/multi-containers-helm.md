## Multi-container pods

multi-container pods can share the same lifecycle, created and destroyed together. They share the same network space, can share volume
adding container infos to pod definition files

design patterns
--------------

* SIDECAR - Logging agent aside a web app
* Adapter - 3 apps producing logs in different formats. the adapter container processes the logs before sending to main logging server.
* Ambassador - app communicates to different DB instances local for dev and prod. connectivity must be modified in the application code. outsource that logic to a separate container in the pod.

at times you may want to run a process that runs to completion in a container. For example a process that pulls a code or binary from a repository that will be used by the main web application. That is a task that will be run only one time when the pod is first created. Or a process that waits for an external service or database to be up before the actual application starts.
the process in the initContainer must run to a completion before the real container hosting the application starts.
You can configure multiple such initContainers as well, like how we did for multi-pod containers. In that case each init container is run one at a time in sequential order.
If any of the initContainers fail to complete, Kubernetes restarts the Pod repeatedly until the Init Container succeeds. However, if the Pod has a restartPolicy of Never, and an init container fails during startup of that Pod, Kubernetes treats the overall Pod as failed.

init containers do not support lifecycle, livenessProbe, readinessProbe, or startupProbe whereas sidecar containers support all these probes to control their lifecycle

#### YAML

Init container
--------------
```angular2html
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']  
```

#### imperative

Container logging
-----------------------

docker container event simulator
`docker run kodekloud/event-simulator`
prints logs, events simulating a web server

run in the background (detached)
`docker run -d kodekloud/event-simulator`

see live logs
`docker logs -f ecf = k logs -f ecf`

if there are multiple containers within the pod, you must specify container name in the command
`k logs -f ecf event-siulator-container`



## Helm

treats k8s object from the ground up, as a package manager
looks at objects as a grouped package (service+deployment+pv/pvc+secret)
helps us treat the app as a whole instead of a bunch of k8s objects

A values.yaml file can declare each customization (settings, volumes, pws)

Chart is used to deploy an application. it will have all the template files for services and values files with the variables
chart.yaml describes the app, resources, version etc.

(Artifact) Hub is the official registry.
additionally, more repos exist, like bitnami 

release is an installation of a chart

#### imperative

identify OS installed
`cat /etc/*release*`

install help package (based on OS, check pre-requisite commands in documentation)
`sudo apt-get install helm`

help
`helm --help`

get env/ version
`helm env`
`helm version`

see releases
`helm list`

install/uninstall application in a single command
`helm install wordpress`
`helm uninstall wordpress`

upgrade
`helm upgrade wordpress`

rollback to previous revision
`helm rollback wordpress`

search for `wordpress` chart package in Artifact hub
`helm search hub wordpress`

add bitnami chart repo in controlplane node
`helm repo add bitnami hhtps:charts..../bitnami`

search joomla package from added repo (see versions)
`helm search repo joomla`

get repositories
`helm repo list`

install drupa chart from bitnami  repo, release: bravo
`helm install bravo bitnami/drupa`\

helm install <release-name> <repo>/<chart-name>

only download but not install
`helm pull --untar bitnami/wordpress`

ls wordpress -> then go to chart, change values file and install
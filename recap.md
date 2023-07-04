docker and container

```
docker run -d -p 8080:80 --name webserver nginx
docker ps
docker images
docker container exec -it  webserver bash
docker stop webserver
docker rm webserver
docker rmi nginx
docker build -t hello-world:v1 .
docker create volume
docker vulume ls
docker compose build
docker compose start
docker compose stop
docker compose up -d
docker compose logs -f web-fe
docker compose down
docker tag my_image k8sacademy/my_image:latest
docker push k8sacademy/my_image:latest
docker pull k8sacademy/my_image:latest
```

### K8s

- service discovery & load balancing
- storage orchestration
- automated rollouts and rollbacks
- self-healing
- secret & configuration management

kubectl - K8s cli, communicate with the apiserver (kube-apiserver):: K8s context

kubectx - quickly switch context

YAML

- apiVersion: Api version of the object
- kind: type of object
- metadata.name:  unique name for the object
- metadata.namespace: scoped env name
- spec: object specs or desired state

namespaces: allow to group resources <dev, test, prod>...

nodes<physical/vm>: group of node=cluster, control plane=master node

worker node: container runtime, kubelet, kube-proxy

pods: atomic of the smallest unit of work of K8s, encapsulates an app's container, represents a unit of deployment

init containers!!

```
kubectl cluster-info
kubectl config current-context: get the current context
kubectl config get-contexts: list all context
kubectl config use-context [contextName]: set the current context
kubectl config delete-context [contextName]: delete all context from the config file
kubectl create -f [YAML file]
kubectl get deploy
kubectl get ns
kubectl config set-context --current --namespaces=[namespaceName]
kubectl create ns
kubectl delete ns
kubectl get pods --all-namespaces: list all pods in all namespaces
kubectl get nodes
kubectl describe node
kubectl get pods -o wide
kubectl describe pod mynginx
kubectl exec -it mynginx -- bash
kubectl delete pod mynginx
kubectl apply -f myapp.yaml
kubectl delete -f myapp.yaml
kubectl get ep myservice: ep = endpoint
kubectl port-forward service/myservice 8080:80
kubectl create -f [pod-definition.yml] : create a pod
kubectl exec -it [podName] -c [containername] --sh
kubectl logs [podName] -c [containername]
kubectl delete -f two-containers.yaml --force --grace-period=0
```

Workloads: an application running on K8s

- Pod
- ReplicaSet: manage pod replicas, ensure the desired number of pods are running > Deployment (pod yml -> template)
- Deployment: ReplicaSet=Self-healing+scalability <> Deployment=Rolling updates+rollbacks
- DeamonSet: node-local facilities, such as storage driver or network plugin [all nodes]
- StatefulSet(kind Service) ???
- Tasks that run to completion: Job, CronJob(spec > schedule): workload for short lived tasks

Updates[Deployment]:

- replicas: number of pod instances
- revisionHistoryLimit: number of previous iterations to keep
- strategy(type): RollingUpdate(maxSurge, maxUnavailable :: def=25%), Recreate
- **Blue-Green Deployments

Services

- ClusterIP: default, Cluster internal, port/targetPort, Load balanced using round robin
- NodePort: extend the ClusterIP, internal/external(Node IP + nodePort), NodePort, port/targetPort
- Loadbalancer: L4 && Ingress: L7: 

Volume

- Storage: Cloud storage provider < CSI < Persistent Volume/PVC, StorageClass
- Storage > Static way: Persistent Volumes[available, bound, released, Failed] and Claims
- Storage > Dynamic way: StorageClass!!

ConfigMaps + Volumes: need to learn more tho

Secrets: can mounting vol too, ?

Observability: Startup, Readiness!, Liveness! probes:: [Readiness: stop accepting traffic/Liveness: restart the pod]

Dashboards: K8s, Lens, ...

Scaling Pods

- Horizontal Pod AutoScaling: use K8s metrics server, pods must have requests and limits defined, scale according to min/max number of replicas, w/ cooldown/delay, HPA checking



```
kubectl get rs
kubectl describe rs rs-example
kubectl delete rs [rsName]
kubectl create deploy [deploymentName] --image=busybox --relicas=3 --port=80
kubectl get deploy
kubectl describe deploy deploy-example
kubectl get ds
kubectl describe ds [dsName]
kubectl delete ds [dsName]
kubectl get sts
kubectl describe sts [stsName]
kubectl delete sts [stsName]
kubectl create job [jobName] --image=busybox
kubectl get job
kubectl desscribe job [jobName]
kubectl delete job [jobName]
kubectl create cronjob [jobName] --image=busybox --schedule="*/1 * * * *" -- bin/sh -c "date;"
kubectl get cj
kubectl desscribe cj [jobName]
kubectl delete cj [jobName]
kubectl rollout status: get the progress of the update
kubectl rollout history deployment [deploymentName]: get history of the deployment
kubectl rollout undo [deploymentName]: rollback a deployment
kubectl rollout undo [deploymentName] --to-revision=[revision#]

kubectl expose po [podName] --port=80 --target-port=8080 --name=frontend: service to expose pod
kubectl expose deploy [deployName] --port=80 --target-port=8080: service to expose deployment
kubectl get svc
kubectl describe svc [serviceName]
kubectl delete svc [serviceName]
kubectl expose po [podName] --port=80 --target-port=8080 --type=NodePort
kubectl expose deploy [deployName] --port=80 --target-port=8080 --type=NodePort

kubectl get pv
kubectl get pvc
kubectl describe pv [pvName]
kubectl describe pvc [pvcName]
kubectl delete pv [pvName]
kubectl delete pvc [pvcName]
kubectl get sc
kubectl describe sc [pvName]
kubectl delete sc [pvName]

kubectl create cm [name] --from-file=myconfig.txt
kubectl create cm [name] --from-file=config/ : folder
kubectl get cm
kubectl get cm [name] -o YAML : save a ConfigMap in a YAML file

kubectl get secrets
kubectl get secrets [secretName] -o YAML
kubectl delete secrets [secretName]

kubectl autoscale deployment [name] --cpu-percent=50 --min=3 --max=10
kubectl get hpa [name]
kubectl delete hpa [name]

```
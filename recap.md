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
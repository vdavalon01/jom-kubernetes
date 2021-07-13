[Configuration](https://www.notion.so/Configuration-d343a7e388744b3790cfedeaf53dce9d)

Core concepts

Nodes(Minions)

- what if node fails
- need more than 1 nodes
- cluster consists of multiple nodes

Master node?

- watch all the nodes in the cluster
- when install k8s on server
- consists api server ,etcd, kubelet, container runtime, controller, scheduler
- api is the front end
- worker node, → container runtime
- kube-apiserver at master
- worker node got kubelet
- kubectl cluster-info

### Pod

Assumptions 

- docker image already exist
- cluster already running
- container are encapsulated inside a pod
- do not add additional container inside your pod to achieve HA
- helper containers - live along side inside the pod

Extract definition file

kubectl get pod <pod-name> -o yaml > pod-definition.yaml

### Replication

how to update replica set

kubectl replace -f repl.yaml

kubectl scale —replicas -f replicaset-definition.yaml

### Deployment

- can be rollback

### namespaces

- kube-system
- default
- kube-public
- if on production, dev use other namespaces
- each namespaces can have their own policies
- each namespaces has dns
- cluster.local domain
- db-service.dev.svc.cluster.local

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3471120b-9317-4f38-98f7-4c1cd1999ff0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3471120b-9317-4f38-98f7-4c1cd1999ff0/Untitled.png)

- kubectl get pods —namepaces=kube-system
- kubectl create -f pod-definition.yaml —namepaces=dev
- kind: Namespace
- kubectl create namespace dev
- to switch permanently
- kubectl config set-context $(kubectl config current-context) —namespace=dev
- kubectl get pods —all-namespaces
- RESOURCE QUOTA

### EXAM TIPS

# POD

**Create an NGINX Pod**

`kubectl run nginx --image=nginx`

**Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)**

`kubectl run nginx --image=nginx  --dry-run=client -o yaml`

# Deployment

**Create a deployment**

`kubectl create deployment --image=nginx nginx`

**Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)**

`kubectl create deployment --image=nginx nginx --dry-run -o yaml`

**Generate Deployment with 4 Replicas**

`kubectl create deployment nginx --image=nginx --replicas=4`

You can also scale a deployment using the `kubectl scale` command.

`kubectl scale deployment nginx --replicas=4`

**Another way to do this is to save the YAML definition to a file.**

`kubectl create deployment nginx --image=nginx--dry-run=client -o yaml > nginx-deployment.yaml`

You can then update the YAML file with the replicas or any other field before creating the deployment.

# Service

**Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379**

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`

(This will automatically use the pod's labels as selectors)

Or

`kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`  (This will not use the pods labels as selectors, instead it will assume selectors as **app=redis.** [You cannot pass in selectors as an option.](https://github.com/kubernetes/kubernetes/issues/46191) ****So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)

**Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:**

`kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml`

(This will automatically use the pod's labels as selectors, [but you cannot specify the node port](https://github.com/kubernetes/kubernetes/issues/25478). You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the `kubectl expose` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

[Multi-container PODs](https://www.notion.so/Multi-container-PODs-32b095cc4636469bacc18da8460e0f1c)

[observability](https://www.notion.so/observability-f31f57c28e59432c91f4842b4de2dfda)

[Pod Design](https://www.notion.so/Pod-Design-1d26ae5768814c139ca7d7e4edb172d9)

[Services and Networking](https://www.notion.so/Services-and-Networking-5a283923bd314c5db5250c9c08c3564b)
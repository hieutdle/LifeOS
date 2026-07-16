
## Kubernetes for the absolute beginners

### Kubernetes Architecture


![[kodekloud-1783612741466.webp]]

API server: CLI, users talk to the API Server to interact with the k8s cluster.
ectd: store all data usage to manage the cluster
Scheduler: distribute works or containers across multiple nodes.
Controller: noticing and responding
Kubelet agent: run on each node, responsible for making sure that the container are nunning on the node as expected
Container Runtime: use to run container

### Master vs Worker node

![[kodekloud-1783613061031.webp]]


### Docker vs container-d

![[kodekloud-1783613280375.webp]]
![[kodekloud-1783613234319.webp]]


### Pods

The containers are encapsulated into a k8s object called Pod. Pod is a single instance of application and a smallest object that you can create in k8s.

![[kodekloud-1783613734046.webp]]

Need to scale application -> don't bring up new container instance within the same pod
-> Create a new pod together with new instance of application
More users + current node no capacity
-> New additional node to expand physical capacity
Pod have one to one relationship with container contains your application

### Multiple-container pods

Need helper containers to support your main app:

![[kodekloud-1783613969389.webp]]

But this a rare use case

### How to deploy pod

```bash
kubectl run nginx --image nginx
```

_As of version 1.18, kubectl run (without any arguments such as_ `--generator` ) will create a pod instead of a deployment.

To create a deployment using imperative command, use **kubectl[]() create:**

`kubectl create deployment nginx --image=nginx`

### Pods with YAML

kind: kind of object we are going to create

kind: Pod | Service | ReplicaSet | Deployment


### ReplicationController and Replicaset

They are the same, the first one is old, and the latter is newer

![[kodekloud-1783692173420.webp]]

#### Key Differences

**Selector Flexibility:**

- Replication Controller uses equality-based selectors.
- ReplicaSet introduces set-based selectors with more flexibility.

**Label Matching:**

- Replication Controller uses `matchLabels` for label matching.
- ReplicaSet uses `matchExpressions` for advanced label matching.

**Updates and Rollbacks:**

- ReplicaSet provides more advanced updating strategies and supports rollbacks.
- Replication Controller has limited updating capabilities.


Make use of `kubectl edit replicaset new-replica-set` or the `kubectl scale replicaset` command.

delete multiple pods with label

```bash
kubectl delete pods -l name=busybox-pod
```

Scale replica set

`kubectl scale replicaset new-replica-set --replicas=4`

`kubectl create deployment httpd-frontend --replicas 3 --image httpd:2.4-alpine`

![[kodekloud-1783794427627.webp]]

![[kodekloud-1783794540639.webp]]

Rolling update is default deployment strategy, so take down and create new one by one

![[kodekloud-1783848911451.webp]]

Each of Pod in each replicaSet will be created or deleted

RollingOut: PODS are upgrade few at a time

ReCreate: All Pods are taken down before upgrading any

To avoid any YAML-based issues, use the command below:

```bash
kubectl set image deploy frontend simple-webapp=kodekloud/webapp-color:v2
```

Difference between `k get deploy, k get deployments, k get deployment?`

Kubernetes accepts all three

![[kodekloud-1783875215076.webp]]

4 pods => so only one Pod can be down simultaneously for an upgrade


Recreate:

```bash
Hello, Application Version: v2 ; Color: green OK

Hello, Application Version: v2 ; Color: green OK

Failed

Failed

Hello, Application Version: v3 ; Color: red OK

Hello, Application Version: v3 ; Color: red OK
```

### Networking in Kubernetes

IP address is assign to a Pod instead of Container like Docker

![[kodekloud-1784033915959.webp]]

10.244.0.0 is internal IP address, so how we get 192.168.1.2

![[kodekloud-1784033943714.webp]]

we have some services that can create virtual network of all pods where they all have unique IP address and do the routing for us



### Services

![[kodekloud-1784034855621.webp]]

want to access the pod? you will have to ssh into 192.168.1.2 and run the curl command inside the network

Kubernetes service an object like replicaset or pods, it listens to a port on the node and forward request on that port to a port on pod that running the web app. This is called **NodePort**.


**ClusterIP**: virtual IP inside a cluster to enable communication between services like backend frontend.

**Load Balancer**: Distribute Load across the different web server.

![[kodekloud-1784035647956.webp]]


#### NodePort
![[kodekloud-1784035875837.webp]]

`ports` is an array, so you can define multiple ports for one service

![[kodekloud-1784036010098.webp]]

Use selector to link the service-nodeport and the target port

![[kodekloud-1784036844645.webp]]

#### ClusterIP

![[kodekloud-1784036974800.webp]]


which one is `10.244.0.2` should connect to?

Kubernetes can help us group service and provide one single interface


![[kodekloud-1784041457142.webp]]

The difference is **where the Service can be reached from**:

| Service type | Accessible from                    | Typical use                                                     |
| ------------ | ---------------------------------- | --------------------------------------------------------------- |
| `ClusterIP`  | Only inside the Kubernetes cluster | Communication between applications/services                     |
| `NodePort`   | Outside and inside the cluster     | Exposing an application through each node’s IP and a fixed port |

A `NodePort` Service also automatically gets a `ClusterIP`. Therefore, it can be accessed both:

```
Inside cluster: frontend-service:80
Outside cluster: <Node-IP>:30080
```

So:

```
ClusterIP = internal access
NodePort = internal access + basic external access
```

For production external access, `LoadBalancer` or `Ingress` is usually preferred over directly using `NodePort`.

#### Load Balancer

![[kodekloud-1784041799856.webp]]

Voting-app Service - 30005 and Result-app are NodePort services

We have three options: VM + nginx

![[kodekloud-1784041902613.webp]]

Supported cloud platform:

![[kodekloud-1784041932983.webp]]

If in unsupported cloud -> same effect as NodePort

| Command     | Resource does not exist | Resource already exists      |
| ----------- | ----------------------- | ---------------------------- |
| `create -f` | Creates it              | Fails with `AlreadyExists`   |
| `apply -f`  | Creates it              | Updates it to match the YAML |


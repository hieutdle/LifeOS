
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


recreate:

```bash
Hello, Application Version: v2 ; Color: green OK

Hello, Application Version: v2 ; Color: green OK

Failed

Failed

Hello, Application Version: v3 ; Color: red OK

Hello, Application Version: v3 ; Color: red OK

```



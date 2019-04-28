### What is Kubernetes
Kubernetes is  a container orhestration framework. It allows you to provision hosts, start containers in a host, restart failed containers, link containers together, expose containers to the rest of the world, and scale up and down.

#### Multi-host container scheduling
- done by the kue-scheduler
- assignes pods to nodes at runtime
- chekcs resources, quality of service, policies and user speifications before scheduling
-
#### Scalability and Availability
- k8s master can be deployed in a highly available configuration
- Multi-region deployments available
- supports 5k node cluster
- 150,000 total pods
- pods can be scaled via an api
-
#### Flexibility and Modularity
- plug and play architecture
- etend architecture when needed
- add ons: network dirvers, service discovery, container runtime, visualization, and command
#### Registration
- new nodes register themselves with master
-
#### discovery
- Automatic deteciton of serviexs and endpoints via DNS or env vars
-
#### Persistent Storage
- mpods can use persistent volumes to store data
- data retained across pod restarts and crashes
-
#### Application Upgrades and Downgrades
- Upgrades:rolling updates supported
- Downgrades: rollbacks are supported
#### maintenance
- hosts can be unscheduled for maintenance
- all apis are versioned
#### Logging
- App monitoring built in
    - tcp, http, or container execution health chekc
- Node health check
    - failures monitored by node controller
- Kubernetes status
    - Add ons like Heapster and CAdvisor
- Logging frameworks
    - in place or extensible (you can use things like ELK)
#### Secrets Management
- sensitive data is a first class citizen
- mounted as data volumes or env vars
- specific to a namespace

### Architecuture
#### Master Node
- the Api Server
    - allows you to interact with the kubernetes control plane
- scheduler
    - watches created pods and designs the pod to run on a specific node
- controller manager
    - node controller runs controllers - backround threads that run tasks in a cluster
    - has a bunch of different roles
- etcd
    - distributed db used for persistent storage
- kubectl
    - cli for interfacing with the API Server
- kubeconfig
#### worker node
- kubelet
    - agent communicates with the api server.
    - see if pods have been assigned
    - execute pod containers via the container engine
    - mounts and runs pod volumes and secrets
    - knows about pod states and responds back to the master
- container platform (eg docker)
- kube-proxy
    - network proxy and load balancer
    - handles network routing for tcp and udp packets
    - performs connection forwarding
    - handles internet traffic
- pod
    - smallest unit to be scheduled
    - group of containers which share storage, linux namespace, ip addresses
    - kubelet and kube-proxy communicates with them

### Hello World
In order to test kubernetes, we will use minikube. Minikube is an app which makes it simple to run kubernetes locally. Of course, you need to install minikube first. And you need to have Kubernetes and docker installed as well as virtualbox.

```
minikube start
```

This will download the iso when run for the first time.

Verify that they are talking to each other

```
>kubectl get nodes

NAME      STATUS    ROLES     AGE     VERSION
minikube  Ready     <none>    31s      v1.8.0
>
```

Now we are ready to run hello world.

We are going to run a common hello world app:

```
>kubectl run hw --image=karthequian/hellowordl --port=80
deployment "hw" created
>kubectl get deployments
NAME    DESIRED    CURRENT  UP_TO_DATE AVAILABLE   AGE
hw      1          1        1          1           8s
>kubectl get rs
NAME      DESIRED   CURRENT  READY  AGE
hw-63636  1         1        1      32s
>kubectl get pods
NAME      READY     STATUS    RESTARTS   AGE
hw-asfsaf 1/1       Running   0          1m
```

now we need to expose this as a service

```
>kubectl expose deployment hw --type=NodePort
service "hw" exposed
```
Take a look

```
>kubectl get services
NAME        TYPE      CLUSTER-IP    EXTERNAL-IP     PORT(S)       AGE
hw          NodePort  10.0.0.252    <none>          00:32611/TCP  6s
kubernetes  ClusterIP 10.0.0.1      <none>          443/TCP       51m
```

See app up and running we need to tell minikube to pull this up in the browser

```
>minikube service hw
```

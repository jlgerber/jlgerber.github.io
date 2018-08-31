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
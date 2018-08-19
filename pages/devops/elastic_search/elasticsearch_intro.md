# Elasticsearch Intro

## 3.1 Clustering and Configuration Strategies
### Traditional SQL DBS
- have database
- has tables
- resides on a server
- might have a replica database to help with scaling

### Elasticsearch built to scale horizontally ( add nodes )
- Elasticsearch index
- add nodes as needed

### how is data stored
#### Index
Data is stored in an index in Elasticsearch. You might think of an index like a database. Contains all of the json documents you add to it plus metadata.
Adding data to an index is called indexing.

#### Shard
Indexes are stored in shards. You must have one or more indexes in a shard. Each shard is a Lucene instance. Indexes may live across multiple shards.

A node may house multiple shards.

#### Nodes and rebalancing
Each physical server is a node.
When you add a new node to the cluster, nodes "gossip" to exchange information then Elasticsearch automatically migrates shards to balance the load between nodes.

#### Replicas
A duplicate of a shard. It acts as a backup of a shard. They can exist across nodes. They are for redundancy and to a certain extend server capacity. Replicas can serve data. However, they are not written to directly. If you have redundant replicas, if a node drops out, you may see no effect.

### Node Failures
If a node goes off line, elasticsearch will attempt to rebalance replicas and shards across the remaining nodes.

## Node Roles
A node can be configured to be one of three things - a data node, a master node, or a client node. ( or all 3 )
### data node
- workhorse of the cluster.
- physically houses the shards of data.
- does not directly serve query requests.
- tends to have the beefiest resource requirements in terms of disk, cpu, etc.

### master node
- brains of the operation
- in charge of maintaining the cluster state. ( all nodes have a copy of the cluster state)
- only the master node is allowed to update the state
- without a master, the cluster won't function

### client node
- gateway to the cluster.
- handles query requests and directs them to data nodes
- don't require as much in terms of resources as data nodes

## Capacity Planning
How many nodes to create for the cluster. It is a tricky business. How does one estimate needs?

### determining the number of data nodes
- set up a load test on one node. insert number of test docs you think you need and record performance metrics. Rinse, and repeat. Divide response time by number of data nodes to get ideal response time and that is the number of data nodes you need.

### determining number of master nodes
If you have one master, it goes offline, and the cluster elects a new master. And then the original master comes back online, you end up with two masters and a "split brain" scenario, which is not good. Both masters think that they are in charge. You could realize dataloss.
There is a setting in the config called:
```minimum_master_nodes: x```
Now many master nodes must be present to elect a master.
```Good Rule of Thumb:take number of master elegible nodes / 2 + 1```
EG:
If there are 10 data nodes then you would need 10 / 2 + 1 or 6 nodes present.

Or if you have 10 nodes but 3 dedicated master nodes. The quorum would be 2.
#### The elasticsearch team recommends having at least 3 master eligible nodes in your production cluster. Good advice. Use at least 3 dedicated master nodes.

### client nodes
We should add a number of client nodes as well. Probably good idea to put them behind a a load balancer.

## Server Requirements
- physical or virtual?
- cpu
- ram

Lets review recommendation from elasticsearch team.
### cpu
- The more cores the better.
- Favor cores over clockspeed.
- isnt real cpu intensive but it takes advantage of multple cores

### ram
- very important especially on the data nodes
- JVM has heap limitations to consider
- 64 gb is the sweet spot for Data nodes as it is the sweetspot for the JVM
- 32 gb should be fine on the client and master nodes.

### disk
- since Elasticsearch is a database, get the fastest disks possible. SSDs are preferred. Safe to use RAID 0 for more speed. Elasticsearch has built in shard balancing.
- Avoid using network attached storage!. Performance can fall off a cliff.

### Networking
- 1 gig wil lsuffice
- 10 gig may be better if shards are large
- avoid clustering across datacenters or different geographical locations.
### virtual machines or docker
- great for easliy adding nodes- make sure the hosts aren't too busy and slow down vms
- do not use for data nodes in production
- try not to use too many. could casue operations management difficulties

### Final Specs
#### Data Nodes
- hardware
- 64 gig
-4 core cpu
- 4x 1 TB ssd disks in raid 0
Optional: os on its own disk array.

### Master and Client Nodes
- vms
- 32 GB of ram
- 2 cpu cores
- 20 GB disk space
### NB: Avoid putting more than one node of the same role on the same virtual machine host!!!

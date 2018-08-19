### Docker Swarm
Docker swarm mode allows us to extend the ease of use of docker and docker-compose to multiple computers.

#### Load Testing - an aside
Before getting started in ernest, we need to grab a tool or two which will let us determine reasonable deployments for services, based on load testing. We want to know now many instances of a given service we should stand up, and whether a greater number of said services are making a difference. So lets use Apache's ```ab``` command, along with ```parallel```.

##### ApacheBench (ab)
ApacheBench is a tool which allows us to automate restful calls.
First we need something to test
```
docker run --rm -p 3000:3000 -d swarmgs/nodewebstress
```

```
ab -n 1000 -c 4 http://127.0.0.1:3000/customer/1
```
flag | effec
-- | --
-n | numer of calls to make
-c | concurrent calls

##### Running ab from a container
```
https://github.com/devth/alpine-bench
```

```
docker run -it --rm -t devth/alpine-bench -n3 http://<ip>:3000/customer/1
```

##### Testing multple routes
We can use the ```parallel``` command to test the efficacy of multiple app servers.
So, we stnad up a second app server listening on port 3001 and then bomb them poth in parallel.
```
docker run --rm -p 3001:3000 -d swarmgs/nodewebstress

```
And lets test
```
echo "http://192.168.1.10:3000/customer/1\nhttp://192.168.0.10:3001/customer/2" | parallel -j2 "ab -n 1000 {.}"
```
#### Custom Network - linking containers together without using explicit IP addresses

In docker, networks are first class citizens. We can list, inspect, and create them from the command line.

##### list networks
```
docker network ls
```
##### Creating a new network

There are a couple types of networks we can create. For simplicity sake, we are going to create a ```bridge``` network, which is only visible on a single node. Its good for testing. However, ultimately, to connect multiple machines, we are going to want another type of network called an ```overlay``` network.

```
docker network create -d=bridge company
```
###### Testing the network with an app
(we run a container. in this case we dont expose a port so we cannot access it externally)

######### customer api
```
docker run --rm -d  --name customer-api --network company swarmgs/customer
```

######### balance api
We can refer to the customer api by name instead of ip address. This resolves via dns.
```
docker run --rm -d --name balance-api -p 4000:3000 --network company -e MYWEB_CUSTOMER_API=customer-api:3000 swarmgs/balance
```
######### Reading the balance
```
http://localhost:4000/balance/1
```
######### Service discovery
Notice that you can ping other containers by name on the same network, using dns to look up their addresses.

```
docker run -it --rm --network company alpine ping customer-api
```

#### Simplifying with docker-compose
Rather than do all of this by hand, we can use a docker compose file to set this up.

###### docker-compose.yaml
```
version: '3'
services:
    customer-api:
        image: swarmgs/customer
    balance-api:
        image: swarmgs/balance
        ports:
            - 5000:3000
        environment:
            MYWEB_CUSTOMER_API: customer-api:3000
```
Notice that we don't specify a network. By default a bridge network will be created for us...

###### Running the services with docker-compose
```
docker-compose -f ./company.yaml up -d
```
###### Stopping the services
```
docker-compose -f ./company.yaml stop
```
###### Stopping an individual service
```
docker-compose -f ./company.yaml stop customer-api
```
### Setting up docker swarm on a single node

Lets first check to see if swarm mode is active
```
docker info | grep -i swarm
```
#### Viewing Docker subcommands
All subcommands use the ```swarm``` subcommand

```
docker swarm
```

#### Enabling swarm mode on a node

```
docker swarm init
```

I get the following results:
```
docker swarm init
Swarm initialized: current node (lizvi0tt7wvsqgvur3nlyirc9) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0eo3naesn9i4a2xxjcd4mxkb6d6z2egyv28p4fwqo3yeld623l-0hnm7jhy9g4hxa9bfvwq61imf 192.168.65.2:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

#### Managing swam nodes with *docker node*

##### Getting a list of nodes in the swarm

```
docker node ls
```
##### Inspecting a docker swarm node

```
docker node inspect <node id>
```
TO inspect the node  you are on
```
docker node inspect self
```
##### Run a container on the node
if you run a container with ```docker run``` it wont be a part of the swarm. Instead, we need to use ```docker service create```

###### Running nginx in the swarm
```
docker service create --name web -p 8080:80 nginx
```
###### Listing the running services
```
docker service ls
````
###### gettign the service status
```
docker service ps web
```
##### Understanding Services

A service is a definition describing the *desired state* for your application. A service generates one or more tasks and each task generates a container. A service defines things like the image to run, exposed ports, commands, working directory, environment, memory and cpu limits, network, and the number of replicas (expected container instances).

The swarm manager accepts your service definition of the desired state of your application and is responsible for maintaining that state.

###### service modes

######### *replicated* Service Mode
run as many replicated containers as requested, spread throughout the nodes

######### *global* Service Mode
one instance on each node on the cluster
##### Removing a service
```
docker service rm <serice name>
```
eg
```
docker service rm web
```
##### Updating a service
Normally, in production, if you want to tweak a service you do *not* remove it. You update it.

###### *docker service update*
```
docker service update <service> <flags>
```
Eg, to scale the number of nginx nodes to 2 in our current example
```
docker service update web --replicas=2
```
Now check it
```
docker service ps web
```
###### Scaling the number of service tasks
Scaling is such a common requirement that there is a shorthand for this
```
docker service scale <service>=<number>
```
eg
```
docker service scale web=3
```
##### Understanding tasks vs containers
THe docker swarm manager is responsible for keeping the state matching the service definition. Tasks are basically container templates or definitions. We can see the state managemnt in action by killing a container and prodding the system. Lets get a list of containers with ```docker  ps``` and run ```docker stop <id>``` with the id of a web container.
Now lets prod the system
```
docker service ps web
```
You should see the a new container running in place of the old container we stopped, as well as an indication that the container shutdown.

##### Starting a second service

```
docker service create --name customer-api -p 3000:3000 swarmgs/customer
```
checking the state
```
docker service ls
```
and the task state
```
docker service ps customer-api
```
we can go go a web browser and see the service in action
```
http://localhost:3000/customer/1
```
#### Getting Rid of single node swarm
```
docker swarm leave --force
```
### Multi Node Swarm

[github account for docker swarm getting started](https://github.com/g0t4/docker-swarm-mode-getting-started)

##### Using docker-machine to spin up some vms.
```
docker-machine create -d virtualbox m1
```
##### Inspecting the vms via *docker-machine*
```
docker-machine ls
```
###### ssh'ing into a particular virtual machine using
```
docker-machine ssh <machine name>
docker-machine ssh m1
```
##### Destroying a vm
```
docker-machine rm <name>[ <name2>...]
docker-machine rm m1 m4
```
#### Using Vagrant to model a swarm

Rather than use docker-machine, you can use vagrant to set up and configure vms. Here is a vagrant file example which sets up a swarm. put it in a file called **Vagrantfile** and vagrant up.
```

### -*- mode: ruby -*-
### vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "box-cutter/ubuntu1610"
    config.vm.provision "shell", path: "provision/node.sh", privileged: true

    ### Managers
    (1..3).each do |number|
        config.vm.define "m###{number}" do |node|
            node.vm.network "private_network", ip: "192.168.99.20###{number}"
            node.vm.hostname = "m###{number}"
        end
    end

    ### Workers
    (1..4).each do |number|
        config.vm.define "w###{number}" do |node|
            node.vm.network "private_network", ip: "192.168.99.21###{number}"
            node.vm.hostname = "w###{number}"
        end
    end

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 1
    end

end
```


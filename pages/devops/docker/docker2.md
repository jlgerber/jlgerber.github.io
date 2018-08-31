### cgroups
cgroups lets you manage access to underlying hardware by leveraging linux kernel isolation features. Broadly the kernel lets you control resources like memory and  cpu.

You can set up cgroups via the docker run command.

flag | description
--- | ---
-c, --cpu-shares int | CPU shares (relative weight)
--cpus decimal | Number of cpus
--cpuset-cpus string | CPUs in which to allow execution (0-3, 0,1)
--cpuset-mems string | MEMs in which to allow execution (0-3, 0,1)
-m, --memory bytes | Memory limit
--memory-reservation  bytes | Memory soft limit
--memory-swap bytes | Swap limit equal to memory plus swap: '-1' to enable unlimited swap
--memory-swappiness int | Tune container swappiness (0 to 100 default -1)

----

More info can be found at [limiting a container's resources](https://docs.docker.com/config/containers/resource_constraints/)

### upgrading docker

archive the configs for docker

```/var/lib/docker```

update apt and follow the instructions, which generally involves uninstalling and reinstalling...

```
apt-get update
```

### Configuring Docker
#### Docker Storage drivers
If you need persistant data within your container you will need to use a storage driver. To learn what driver you are using you would type:

```
docker info | grep Storage
```

This will list the currently configured storage driver. However, one has choices and a good place to start is here ( [Docker Storage Drivers](https://docs.docker.com/storage/storagedriver/select-storage-driver/) )

#### Docker Registries, Repositories, and Tags

Docker images are identified by unique tags, sotred in repositories, optionally tracked by registries. As a reminder here are some definitions (thanks stackoverflow)

##### Registry

A service responsible for hosting and distributing images. The default registry is the Docker Hub.

##### Repository

A collection of related images (usually providing different versions of the same application or service).

##### Tag

An alphanumeric identifier attached to images within a repository (e.g., 14.04 or stable ).

So the command ```docker pull amouat/revealjs:latest``` will download the image tagged latest within the ```amouat/revealjs``` repository from the Docker Hub registry.

### Checking Status of Docker
Can we communicate with the server? what version of the client and server do we have?

```
docker version
```

How many containers we have on a host. How many containers running vs stopped, etc
```
docker info | more
```

what containers are running on the host
```
docker ps
```

what containers are both active and inactive?
```
docker ps -a
```

On a swarm manager, you can get the status of the swarm cluster
```
docker node ls
```

#### Configuring docker logging
1. default log type per docker daemon
2. per container log types which can override the default

You can find all the details here ()

To determine what the current setup is:

```
docker info | grep 'Logging Driver'
```

To change the default logging type edit daemon.json
```
cd /etc/docker
vi daemon.json
```

Configuring logging for a specific container
```
docker run --log-driver=syslog --log-opt syslog-address=udp://1.1.1.1 alpine
```


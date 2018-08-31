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

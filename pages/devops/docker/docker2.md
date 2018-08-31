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

More info can be found at [limiting a container's resources](https://docs.docker.com/config/containers/resource_constraints/)

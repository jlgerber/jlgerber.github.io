### Docker Commands

  command | description
  --- | ---
  run   |  run a container
  logs   |   view logs for a container
  ps     |   view process information for a container
  commit |   saves changes in a container as a new image
  exec   |   start process wihtin a detached container
  rm     |   delete a stopped process
  rmi    |   delete an image
  start  |   start a stopped container process
  stop   |   stop a running container process

#### run ( run a container with an optional command )

run a command using a container. If you don't specify a command to run,
it will use the default command specified on the docker image.

##### Flags

flag | description
--- | ---
-d | run in detached mode. also known as running as a daemon
  -c  |  count. number of times to run a command
  -P   | publish all exposed ports to random ports
  -p \[host port\]:\[container port\] |   publish a container's port(s) to the host

#### ps ( list container processes )

List running containers

  flag | description
--- | ---
  -a  | list all of the containers ever run

#### logs ( inspect logs of running container )

Inspect the logs of a running container. Useful to monitor what detached
containers are doing.

  flag | description
  --- | ---
  -f  | attach to log. like using tail.

#### commit ( create a container image )

##### Syntax

docker commit \[options\] \[container ID\] \[repository:tag\]

###### Repo name shoudl be based on username/application

###### Can reference the container with the name instead of ID

###### Will use "latest" tag if not supplied.

###### Example

####### Part 1 - run an image and modify it

``` {.example}
docker run -it ubuntu bash
apt-get update
apt-get install -y curl
exit
```

####### Part 2 - find the image commit the image

``` {.example}
docker ps -a
docker commit 9245327 jgerber/myapplication:1.0
```

####### Part 3 - run the new image and verify that the command is available

``` {.example}
docker run -it jgerber/myapplication:1.0 bash
```

exec (start process within a detached container )
-------------------------------------------------

start another process within a container

##### syntax docker exec -i -t \[container ID\] \[command\]

##### use this to start a bash shell

docker exec -it a1fo3t333 /bin/bash

##### exiting the command will note terminate the container, because the command was not PID 1.

##### example

``` {.example}
### start up tomcat as daemon
docker run -d tomcat:7
### find its id and run bash
docker ps
docker exec <ID> /bin/bash
...
exit
```

rm (delete stopped containers)
------------------------------

deletes stopped containers

rmi ( delete docker image )
---------------------------

remove images

##### usage

docker rmi \[image ID\] docker rmi \[repo:tag\]

##### example

``` {.example}
docker images
docker
```

tag ( rename a local image repository before pushing it to docker hub )
-----------------------------------------------------------------------

##### syntax

docker tag \[image ID\] \[repo:tag\] docker tag \[local repo:tag\]
\[Docker Hum repo:tag\]

inspect ( display all the details about a container )
-----------------------------------------------------

-   Outputs details in JSON array
-   Use grep to find a specific property

##### Syntax

docker inspect &lt;Container name | ID&gt;

##### Flags

###### --format

containers
==========

Container Commands
------------------

##### Exiting container without killing it

**\*** ctrl-P-Q

##### Exiting container and killing it

###### Type Exit

Container properties
--------------------

##### container id

###### containers can be specified using their ID or name

####### long ID and short ID

####### Short ID and name cna be obtained using `docker ps` command

####### Long ID obtained by inspecting container

##### container name

###### if you don't specify a name, docker will specify one for you.

Container processes
-------------------

-   a container only runs as long as the process from your specified
    docker run command is running
-   your command's process is always PID 1 inside the container

Container images
----------------

a container image is made up of image layers

##### image layers

file systems layered on top of each other.

###### parent image

an image layer below the image in question

###### base image

the lowest or root image

###### docker creates a top writable layer for containers

###### parent images are read only

###### all changes are made at the writeable layer

###### Docker uses COW to copy on write when the user changes something

##### creating new images

images are created using the

Dockerfile
==========

Configuration file that contains instructions for building a docker
image.

Provides a more effective way to build images compared to using `docker commit`
-------------------------------------------------------------------------------

Easily fits into continuous integration and deployment process
--------------------------------------------------------------

comprised of INSTRUCTIONS
-------------------------

specify what to do when building the image

##### Example Dockerfile

``` {.example}
### Example of a comment
FROM ubuntu:14.04
RUN apt-get update
RUN apt-get install vim
RUN apt-get install curl
```

##### RUN instruction

excutes comman on top writable layer and performs a commit of the image

###### multiple RUN instructions may be aggregated using "&&"

####### example of aggregation

``` {.example}
RUN apt-get upate && apt-get install -y \
curl \
vim \
openjdk=7-jdk
```

##### CMD instruction

-   CMD defines a default command to execute when a container is created
-   CMD performs no action during the image build Shell format and EXEC
    format
-   Can only be specified once in a Dockerfile
-   Can be overridden at runtime by specifying explicitly

###### Shell format cmd args

CMD ping 127.0.0. -c 30

###### Exec Format CMD \["CMD",args...\]

CMD \["ping","127.0.0.1", "-c", "30"\]

##### ENTRYPIONT instruction

-   Defines athe command that will be run when the container is executed
-   Unlike CMD, it cannot be overridden.
-   Arguments specified from CMD or command line, are passed
    as arguments.
-   ENTRYPOINT must be formed using EXEC form in order to pass in args
-   Container essential runs as an executable

##### EXPOSE instruction

-   Configures which ports a container will listen on at runtime
-   Ports still need ot be mapped when container is executed

###### Example Dockerfile

``` {.example}
FROM ubuntu:14.04
RUN apt-get update
run apt-get install -y nginx

EXPOSE 80 443

CMD["nginx", "-g", "daemon off;"]
```

Docker Build with Dockerfile
----------------------------

##### Syntax

docker build \[options\] \[path\]

##### Docker searches for the Dockerfile in the root of the context.

\${path}/Dockerfile

##### Tag the build (-t)

docker build -t \[repository:tag\] \[path\]

###### Examples

####### Build an image using the current folder as the context path. Put image in jgerber/myimage repository and tag it as 1.0

docker build -t jgerber/myimage:1.0 .

####### As above but use the myproject folder in as the context path

docker build -t jgerber/myimage:1.0 myproject

##### Specify a different dockerfile (-f)

Tell docker to search for a different dockerfile name

Building an image from a Dockerfile
-----------------------------------

##### create a directory to house the Dockerfile

##### create a Dockerfile in the directory

``` {.example}
FROM ubuntu:14.04
RUN apt-get update && apt-get install -y curl vim
```

##### run docker build

docker build -t jgerber/testimage:1.0 .

Managing Images and Containers
==============================

Start and Stop Containers
-------------------------

##### docker start &lt;container ID&gt;

###### example

run a docker image detached ( as a daemon )

``` {.example}
docker run -d nginx
```

##### docker stop &lt;container ID&gt;

###### example

stop a detached image

``` {.example}
### find container id
docker ps
### stop container ( example id)
docker stop a23235s
```

Volumes
=======

A volume is a designmated directory in a container designed to persist
data independent of a container's life cycle.

-   Volume changes are excluded when updating an image
-   Perisist when a container is deleted
-   Can be mapped to a host folder
-   Can be shared between containers

Mount a Volume
--------------

-   Volumes are mounted when creating or executing a container
-   Can be mapped to ahost directory
-   Volume paths specified must be absolute

##### Howto mount a volume - examples

###### Execute a new container and moutn the folder /myvolume in its file system

``` {.example}
docker run -d -P -v /myvolume nginx:1.7
```

###### Execute a new container and map the /data/src folder from the host into the /test/src folder in the container

``` {.example}
docker run -i -t -v /data/src:/test/src nginx:1.7
```

Volumes in Dockerfile
---------------------

-   VOLUME instruction creates a mount point
-   Can specify arguments JSON array or string
-   Cannot map volumes to host directories
-   Volumes are initialized when the container is executed

##### String example

``` {.example}
VOLUME /myvol
```

##### String example with multiple volumes

``` {.example}
VOLUME /www/website1.com /www/website2.com
```

##### JSON example

``` {.example}
VOLUME ["myvol", "myvol2"]
```

What are volumes for?
---------------------

-   decouple data that is stored from the container which created the
    data
-   share data between containers
    -   can setup a data container which has a volume you mount in other
        containers
-   Note that binding data to a host is good for testing purposes, but
    has issues because you are directly binding to a real host. ( might
    be fine for us because we control our environment )

Container networking basics
===========================

Mapping ports
-------------

-   Containers have their own network stack. ie their own IP address
-   Maps exposed container ports to ports on the host machine
-   Ports can be manually mapped (via -p) or auto mapped ( via EXPOSE in
    Dockerfile and -P flag)
-   Use -p and -P parameters in docker run

##### Example

Map port 80 on the container to 8080 on the host

``` {.example}
docker run -d -p 8080:80 nginx:1.7
```

Linking Containers
------------------

Linking is a communication method between containers which allows them
to securely transfer data from one to another

-   Source and recipient containers
-   Recipient containers have access to data on source containers
-   Links are established based on container names

##### creating a link

1.  Create the source container first
2.  Create the recipeint container and the --link option

###### example

Create the source container using postgres

``` {.example}
docker run -d --name database postgres
```

Create the recipient container and link it

``` {.example}
docker run -d -P --name website --link database:db nginx
```

##### Uses of Linking

-   Containers can talk to each other without having to expose ports to
    the host
-   Essential for micro service applicaiton architecture
-   Example:
    -   Container with Tomcat running
    -   COntainer with MySQL running
    -   Application on Tomcat needs to connect to MySQL

Administration
==============

Troubleshooting Containers
--------------------------

##### If your container shuts down unexpectedly, it may be because the applicaiton your container was running has crashed.

##### Container PID 1 process output can be viewed with `docker logs` command

##### `docker logs` will show whatever PID 1 writes to stdout.

###### Examples

####### View the output of the containers PID 1 process:

``` {.example}
docker logs <container name | ID>
```

####### View and follow the output

``` {.example}
docker logs -f <container name | ID>
```

####### Tail the last line of the logfile and follow it

``` {.example}
docker logs -f --tail 1 <container name | ID>
```

##### Container application logs

-   typically, apps have a well defined log location
-   Map a host folder to the application's log folder in the container
-   In this way, you can view the log generated in the container from
    your host folder

###### You can mount a volume to map the log folder in the host to the log folder in the container.

####### Example - run a container using nginx image and mount a volume to map the /nginxlogs folder in the host to the /var/log/nginx folder in the container

``` {.example}
docker run -d -P -v /nginxlogs:/var/log/nginx nginx
```

##### docker inspect

You can use docker inspect to see information about a container image.

##### Docker Daemon

###### Starting and stopping Docker daemon

There are many reasons to stop and start the daemon, including changing
logging levels.

-   If you started Docker as a service, use service command to stop,
    start, and restart the Docker daemon
    -   sudo service docker stop
    -   sudo service docker start
    -   sudo service docker restart
-   if not running as a service, run Docker executable in daemon mode to
    start the daemon

    ``` {.example}
    sudo docker -d &
    ```

-   If not running as a service, send a SIGTERM to the Docker process to
    stop it
    -   Run pidof docker to find the Docker process PID
    -   sudo kill \$(pidof docker)

###### Docker Daemon upstart configuration file

-   Located in `/etc/default/docker`
-   use DOCKER~OPTS~ to control the startup options for the daemon when
    running as a service
-   Restart the service for changes to take effect

    ``` {.example}
    DOCKER_OPTS="--log-level debug --insecure-registry myserver.org:5000"
    ```

###### Docer daemon logging

####### start the docker daemon with --log-level parameter and specify the logging level

####### Levels are:

-   Debug
-   Info
-   Warn
-   Error
-   Fatal

####### Example - Run docker daemon with debug log level (log written on terminal)

\###+begin~example~ sudo docker -d --log-level=debug

\###+end~example~

####### Example - Configuring in DOCKER~OPS~ ( log output will be written to /var/log/upstart/docker.log)

\###+begin~example~ DOCKER~OPTS~="--log-level debug"

\###+end~example~ Then run docker as a service

Overview of Security Practices
------------------------------

##### Linux containers and security

-   Docker helps make applications safer as it provides as reduced set
    of default privileges and capabilities.
-   Namespaces provide an isolated view of the system. Each container
    has its own:
    -   IPC, network stack, root file system, etc
-   Processes running in one container cannot see and effect processes
    in another container
-   Control Groups (Cgroups) isolate resource usage per container
    -   ensures that a compromised container won't bring down the entire
        host by exhausting resources

##### Quick security considerations

-   Docker daemon needs to run as root
-   ONly ensure that trusted users can control the Docker daemon
    -   Watch who you add to docker group
-   If binding the daemon to a TCP socket, secure it with TLS
-   Use linux hardening solution
    -   Apparmor
    -   SELinux
    -   GRSEC
-   Docker security is additive and simple and should be used alongside
    best practices for Securing Linux distributions.

###### Further Reading

-   <http://docs.docker.com/articles/security/>
-   <http://www.slideshare.net/jpetazzo/docker-linux-containers-lxc-and-security>

Private Registry
----------------

-   Allows you to run your own registry instead of using Docker Hub
-   Multiple options
    -   Run registry server using container
    -   Docker Hub Enterprise

##### Two versions

-   Registry v1.0 for Docker 1.5 and below
-   Registry v2.0 for Docker 1.6

##### Setting up a private registry

-   Run the registry server inside a container
-   Use the registry image at
    <https://registry.hub.docker.com/u/library/registry/>
-   Image contains a pre-configured version of registry v2.0

###### Example - Run a new container using the registry image

``` {.example}
docker run -d -p 5000:5000 registry:2.0
```

##### Push and pull from a private registry

-   first tag the image iwth host IP or domain of the registry server,
    then run `docker push`

###### Example

``` {.example}
### Tag image and specify the registry host
### myserver.net:5000 <- host domain of registry server:port
### my-app:1.0 <- image repository:tag
docker tag <image ID> myserver.net:5000/my-app:1.0

### push image to registry
docker push myserver.net:5000/my-app:1.0

### pull image from registry
docker pull myserver.net:5000/my-app:1.0
```

So...

``` {.example}
### get list of image repositories via ps
docker images
###  lets take postgres (IMAGE postres)
docker tag 7ee9d2061970 localhost:5000/mypostgres:1.0

### run docker images to find new tag
docker images

### run docker push to push into local registry container
docker push localhost:5000/mypostgres:1.0
```

##### Accessing registry from other computers

-   If you try to pull an image from our repo from another host, you
    will get an error:

    \###+begin~example~

docker pull 192.168.1.3:5000/mypostgres:1.0

\###+end~example~

-   You need to set up TLS or
-   update the host into the insecure registry list in the docker
    daemon's startup parameters
    -   stop daemon
    -   open file /etc/default/docker
-   modify DOCKER~OPS~ option and add --insecure-registry
    &lt;ipaddress&gt;:&lt;port num&gt;

check images in registry
------------------------

##### use curl

curl -v -X GET <http://localhost:5000/v2/postgres/tags/list>

Further Resources
-----------------

-   <https://github.com/docker/distribution>
-   <https://docs.docker.com/registry/overview/>

Orchistration Tools
===================

Intro to Docker Machine
-----------------------

Intro to Docker Swarm
---------------------

Intro to Docker Compose
-----------------------

Building micro service applicaitons with Docker
-----------------------------------------------

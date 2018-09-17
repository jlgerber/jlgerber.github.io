- [FROM Instruction](#from-instruction)
- [ENV instruction](#env-instruction)
- [ARG Instruction](#arg-instruction)
- [RUN Instruction](#run-instruction)
    - [RUN Instruction Best Practices](#run-instruction-best-practices)
        - [Example](#example)
- [COPY Instruction](#copy-instruction)
    - [Example](#example)
- [Defining what a container executes when it starts](#defining-what-a-container-executes-when-it-starts)
    - [CMD Instruction](#cmd-instruction)
    - [ENTRYPOINT Instruction](#entrypoint-instruction)
        - [Example](#example)
            - [CMD Example](#cmd-example)
            - [ENTRYPOINT Example](#entrypoint-example)
            - [CMD with ENTRYPOINT Example](#cmd-with-entrypoint-example)
- [HEALTHCHECK Instruction](#healthcheck-instruction)
- [ONBUILD Instruction](#onbuild-instruction)
- [METADATA instructions](#metadata-instructions)
- [Example - Authoring an Nginx Docker Image](#example---authoring-an-nginx-docker-image)
    - [Nginx](#nginx)
        - [Plan the content](#plan-the-content)
        - [Dockerfile Take 1](#dockerfile-take-1)
            - [Build the dockerfile](#build-the-dockerfile)
            - [how big is the image?](#how-big-is-the-image)
            - [run it](#run-it)
            - [Have We Achieved the Best Outcome?](#have-we-achieved-the-best-outcome)
        - [Dockerfile Take 2 - Multi-stage Docker Image Builds](#dockerfile-take-2---multi-stage-docker-image-builds)

# FROM Instruction

specify base image

```
FROM <repository>[<:tag>]
FROM <repository>[<@digest>]
```

First instruction must be a from instruciton
keyword ```scratch``` indicates build with no base image
The argument must defien a repository but a tag is optional. If no tag is supplied docker assumes that the tag is latest.

Argument may refer to a digset. This is the digest of the images manifest.
You can retrieve the digest like so:
```
docker image inspect --format '{{.RepoDigests}}' <image name>
```

What is the purpose of referencing by digest? A tag may get replaced. A digest is effectively immutable.

# ENV instruction
Docker provides the ENV instruction to retrieve an env var during the build process. There are two forms:
```
ENV <variable> <value>
ENV <variable=value>
```
The variable must be assigned a value.

The scope applies from the point of declaration.

Variable and its value persist into the derived container.

Multiple variables can defined simultaneously like so:
```
ENV MONGO_MAJOE 3.4
ENV MONGO_VERSION 3.4.4
ENV MONGO_PACKAGE mongodb-org
```
However, when using the variable=value form, one may declare multiple variables via the same instruction, which is more efficient:
```
ENV MONGO_MAJOR=3.4 \
    MONGO_VERSION=3.4.4 \
    MONGO_PAXKAGE=mongodb-org
```

# ARG Instruction
Defines a variable passed on the command line during the build.
```
ARG <variable[=default value]>
```
ARG can optionally define a default value

Variable can only be consumed from the point of definition.

Variables do **not** persist into derived container.

Altered build args break build cache at point consumed.

Variables defined with ENV override variables defined with ARG. A variable defined with ARG can be redifined using ENV. Thus a build argument can be passed on to derived containers.

```
# Define build argument for verision
ARG VERSION

# define variable to use to peg version. provide a default value if version sint set
ENV VERSION=${VERSION:-3.5.1}
```

# RUN Instruction
RUN executes command inside container.
```
RUN <command parameter ...>
RUN <["executable", "parameter",...]>
```
Two forms of the syntax: shell and exec.

Shell form executes command in shell.

The exec form is used when filesystem is devoid of a shell. Uses json syntax.

Build cache breaks only if instruction alters.

## RUN Instruction Best Practices
One must be careful not to create an excessive number of layers.

### Example
```
RUN apt-get update
RUN apt-get install -y wget
RUN rm -rf /var/lib/apt/list/*
```
(48mb)
The third RUN command attempts to clean up from the first instruction. However, because it is executed as a separate RUN command, it will not clean up the layering and the resulting image will be larger. We can mitigate this using multiple commands on a single RUN command.

```
RUN apt-get update && \
    apt-get install -y wget && \
    rm -rf /var/lib/apt/lists/*
```
(40.8 mb)

# COPY Instruction
Copy content from the build context into the image.
```
COPY <src> ... <dst>
COPY ["<src>"... "<dst>"]
```
Multiple sources can be specified in one instruction. They will be copied to the same destination directory.

The definition of the soruce can contain globbing characters

The destination can be a relative or absolute path.

Content is added with a UID and GID of 0.

NB: there is also an older, ADD instruction which should be avoided.

## Example
```
# File or directory called foo copied as bar
COPY foo /bar

# File called foocopied as /bar/foo dorectory foo copied as /bar
COPY foo /var/

# File or directory called foo copied as bar
COPY path/foo /bar

# All files or directories located at path, copied to directory /bar
COPY path/tmp* /bar/

# File or direcotry called foo copied as bar, located relative to previous WORKDIR instruction
COPY foo bar
```

# Defining what a container executes when it starts
There are two instructions that may work together or independently.
## CMD Instruction
define a default command.
Or, default parameters to ENTRYPOINT
```
CMD <command parameter ...> or <parameter parameter ...>
CMD ["command>", "<parametr">, ...]
```
Typically you use a CMD instruction when you want to define a default instruction to be used if the user doesn't specify a command, but allow for the user to execute an other command.

Two forms of syntax: shell and exec (preferred)

Exec form used for default parameters

Command line arguments override CMD

## ENTRYPOINT Instruction
```
ENTRYPOINT <executable parameter ...>
ENTYRPOINT ["<executable>", "<parameter>", ...]
```
ENTRYPOINT used for defining an executable.

Employed to constrain what is executed.

Command line arguments appended.

Two forms of syntax: shell and exec (preferred)

Shell form limits controlusing Linux signals. Using the shell, the shell becomes pid 1. Using the exec form, the command itself becomes pid 1.

### Example
#### CMD Example
Dockerfile
```
FROM debian::jessie-slim

RUN apt-get update   && \
    apt-get install -y --no-install-recommends \
        cowsay  \
        screenfetch && \
        rm -rf /var/lib/apt/lists/*

ENV PATH "$PATH:/usr/games"

CMD ["cowsay", "To improve is to change: to be perfect is to change often"]
```

Lets build the image
```
docker image build -t demo .
```
lets run the container with defaults

```
docker container run demo
```

We get a cow.
Because we used a CMD, we can override the command.

```
docker container run demo screenfetch -E
```

#### ENTRYPOINT Example

If we replace the CMD instruction with an ENTRYPOINT instruction in the Dockerfile:
```
FROM debian::jessie-slim

RUN apt-get update   && \
    apt-get install -y --no-install-recommends \
        cowsay  \
        screenfetch && \
        rm -rf /var/lib/apt/lists/*

ENV PATH "$PATH:/usr/games"

ENTRYPOINT "cowsay"
```

Rebuild the image
```
docker image build -t demo .
```
Now execute the container
```
docker container run demo
```
we get a cow with an empty container.

We can supply arguments:

```
docker container run demo -f tux "if you're going through hell: keep going"
```

We cannot override the command itself.

#### CMD with ENTRYPOINT Example
We will make a change to the dockerfile to see how CMD and ENTRYPOINT work together:

```
FROM debian::jessie-slim

RUN apt-get update   && \
    apt-get install -y --no-install-recommends \
        cowsay  \
        screenfetch && \
        rm -rf /var/lib/apt/lists/*

ENV PATH "$PATH:/usr/games"

CMD ["-f", "tux", "Im a panguin dude"]

ENTRYPOINT ["cowsay"]
```

And rebuild:
```
docker image build -t demo .
```
We can run it with arguments:
```
docker container run demo "If you're going through hell; keep going"
```
And without:
```
docker container run demo
```
In the second case, the arguments specified in the CMD are appended to the ENTRYPOINT

# HEALTHCHECK Instruction
```
HEALTHCHECK [opitons] CMD <command>
HEALTHCHECK NONE
```
instruction defines command to test container health.
exit status of the command translates into health states: 0 - for healthy 1 - for unhealthy.

Command runs periodicallly in the container

Options for interval, timeout and retries.

Health status is available via Docker CLI.

# ONBUILD Instruction

Provides a means to impose method on image use

ONBUILD defers exection of instruciton

Trigger is added to the image's metadata

Image used as base image for similar images

For all instructions execept FROM and ONBUILD

# METADATA instructions
A number of additional commands exist to apply metadata to a container. These are

Instruction | Purpose
--- | ---
EXPOSE | Specifies TCP/UDP ports for a container
LABEL | Adds a static label to the image. Useful for anotating an image. But can be used for a variety of purposes.
STOPSIGNAL | Defines the signal to stop a container's process. By default, Docker sends the SIGTERM signal to a container's process to stop it. However some applications are configured to listen to a different signal, so STOPSIGNAL allows us to sent a different signal.
USER | Sets the container's user. If our app doesn't require root privilages, we can specify a non-root user to run the volume.
VOLUME | Specifies a mount point for persistent data. Docker volumes bypass docker's copy on write system and allows us to persist data.
WORKDIR | Sets the working directory

# Example - Authoring an Nginx Docker Image
- Plan the image content
- Author a Dockerfile
- Make use of multi-stage image builds
- Apply multi-stage build for image

## Nginx
Open source http server and reverse proxy written in C. There are numerous docker images which already expose Nginx. However, we want to learn how to do this ourselves.

### Plan the content
- prep - make sure we have correct tooling
- acquire sourcecode
    - download sourcecode
    - verify download (get pgp)
    - unpack sourcecode
- build nginx
    - configure build
    - Make binary
    - Clean
- configure
    - Ensure logging works
    - Provide content
    - Customize image
- serve binary
    - Define Execution

### Dockerfile Take 1
```dockerfile
FROM alpine:latest

# Define build argument for version
ARG VERSION=1.12.0

# use -x shell command to print each command as executed
RUN set -x    && \
    apk add --no-cache --virtual .build-deps \
        build-base   \
        gnupg        \
        pcre-dev     \
        wget         \
        zlib-dev  && \
# retrieve verify and unpack nginx source \
    TMP="$(mktemp -d)" && cd "$TMP" && \
    gpg --keyserver pgp.mit.edu --recv-keys \
        B0F4253373F8F6F510D42178520A9993A1C052F8 && \
    wget -q http://nginx.org/download/nginx-${VERSION}.tar.gz && \
    wget -q http://nginx.org/downlaod/nginx-${VERSION}.tar.gz.asc && \
    gpg --verify nginx-${VERSION}.tar.gz.asc                      && \
    tar -xf nginx-${VERSION}.tar.gz && \
#build and install nginx \
    cd nginx-${VERSION} && \
    ./configure \
        --with-id-opt="-static" \
        --with-http_sub_module && \
    make install && \
    strip /usr/local/nginx/sbin/nginx && \
#Clean up \
    cd / && rm -rf "$TMP" && \
    apk del .build-deps && \
#Configure \
# symlink access and error logs to /dev/stdout and /dev/stderr \
# in order to make use of Docker's logging mechanism \
ln -sf /dev/stdout /usr/local/nginx/logs/access.log && \
ln -sf /dev/stderr /usr/local/nginx/logs/error.log

# Customize static contenxt and configuraiton
COPY ingex.html /usr/local/nginx/html/
COPY nginx.conf /usr/local/nginx/conf/

# nginx needs to receive a sigquit to stop
STOPSIGNAL SIGQUIT

# Expose port
EXPOSE 80

# Define entrypoint and default parameters
ENTRYPOINT ["/usr/local/nginx/sbin/nginx"]
# start nginx in the forground
CMD ["-g", "daemon off;"]
```
#### Build the dockerfile
```shell
docker image build -t nginx
```
#### how big is the image?
```
docker image ls nginx
```
#### run it
```
docker container run -d -p 80:80 nginx
```
#### Have We Achieved the Best Outcome?
- we build a dockerimage for nginx, heavily optimized with size in mind
- This carries a cost in terms of readability, maintainability and productivity as we have had to concat all commands into a single run instruction. there is no build caching
- How can we get teh best of both worlds for our docker image? Split our image in two - **build image** and **service image**
- Docker has multi-stage Image Builds.
### Dockerfile Take 2 - Multi-stage Docker Image Builds
- Multi-stage builds use multiple FROM instructions
- COPY instructions can reference content from a previous build stage
- A build stage is referenced by index, or by a supplied name

```Dockerfile
FROM alpine:latest as build

# Define build argument for version
ARG VERSION=1.12.0

# install build tools, libraries and utilities
RUN apk add --no-cache --virtual .build-deps \
        build-base   \
        gnupg        \
        pcre-dev     \
        wget         \
        zlib-dev
# Retrive, verify and unpack nginx source
RUN set -x                                                        && \
    cd /tmp                                                       && \
    gpg --keyserver pgp.mit.edu --recv-keys                          \
        B0F4253373F8F6F510D42178520A9993A1C052F8                  && \
    wget -q http://nginx.org/download/nginx-${VERSION}.tar.gz     && \
    wget -q http://nginx.org/download/nginx-${VERSION}.tar.gz.asc && \
    gpg --verify nginx-${VERSION}.tar.gz.asc                      && \
    tar -xf nginx-${VERSION}.tar.gz

WORKDIR /tmp/nginx-${VERSION}

# Build and Install nginx
RUN ./configure                          \
        --with-ld-opt="-static"          \
        --with-http_sub_module        && \
    make install                      && \
    strip /usr/local/nginx/sbin/nginx

# Symlink access and error logs to /dev/stdout and /dev/stderror in order
# to make use of Docker's logging mechanism
RUN ln -sf /dev/stdout /usr/local/nginx/logs/access.log  && \
    ln -sf /dev/stderr /usr/local/nginx/logs/error.log

FROM scratch

# Customize staticcontent and config
# the first copy command copies between docker build stages. the build refers to
# our first FROM instruction, specifically the "as build" part.
COPY --from=build /usr/local/nginx /usr/local/nginx
# the following two commands are necessary because we are building from scratch.
# nginx has certain configuration dependencies which we have to fulfill. It expects
# a "nobody" user for instance ( hence copying the passwd and group files)
COPY --from=build /etc/passwd /etc/group /etc/
COPY --from=build /usr/local/nginx /usr/local/nginx
# copy static context
COPY index.html /usr/local/nginx/html/
COPY nginx.conf /usr/local/nginx/conf/
# Change default stop signal from SIGTERM to SIGQUIT
STOPSIGNAL SIGQUIT

# Expose port
EXPOSE 80

# Define entrypoint and default
ENTRYPOINT ["/sr/local/nginx/sbin/nginx"]
CMD ["-g","daemon off;"]
```




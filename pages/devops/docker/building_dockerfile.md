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

Variable can only be consumed from the point of definitionl=.

Variables do **not** persist into derived container.

Altered build args break build cache at point consumed.

Variables defined wit hENV overridevariables defined with ARG. A variable defined wit hARG can be redifined using ENV. Thus a build argument can be passed on to derived containers.

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

### Example
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

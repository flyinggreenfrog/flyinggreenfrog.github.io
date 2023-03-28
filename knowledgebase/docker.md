---
title: Docker
last-changed: 2023-03-28.
knowledgebase: true
categories: [Container, Linux]
---
## Links

* [Docker overview](https://docs.docker.com/get-started/overview) <time>2020-06-22</time>
* [Get started with Docker](https://docs.docker.com/get-started) <time>2019-06-22</time>
* [Docker Compose](https://docs.docker.com/compose) <time>2020-06-05</time>

## Setup

```console
# yum install -y yum-utils device-mapper-persistent-data lvm2
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# yum install docker-ce docker-ce-cli containerd.io
# systemctl enable docker
# systemctl start docker
```

To avoid prepending `sudo` in front of every `docker` command, add your user to
the `docker` group. Be aware of the security implications, as the `docker`
group grants rights equivalent to `root`.

```console
# usermod -a -G docker <USER>
```

## Overview

### Docker architecture

* Docker daemon
  - Docker Engine supports swarm mode in Docker 1.12 and higher
* Docker client
* Docker registries
  - Docker Hub: publie registry
  - Docker Datacenter (DDC) includes Docker Trusted Registry (DTR)
* Docker objects
  - images: read-only template of container
  - containers: runnable instance of image
  - networks
  - volumes
  - plugins
* hierarchy
  - containers
  - services
  - swarms
  - stacks

* `Dockerfile`
* `docker-compose.yml`
* `/var/lib/docker`

### Technologies

* Namespaces
  - pid
  - net
  - ipc (InterProcess Communication)
  - mnt
  - uts (Unix Timesharing System)
* Control groups / cgroups
* Union file systems / UnionFS
  - AUFS
  - btrfs
  - vfs
  - DeviceMapper
* Container format
  - libcontainer

## Usage

### Docker

Getting information:

```console
$ docker -v|--version
$ docker version
$ docker info
```

Getting help:

```console
$ docker <CMD> --help
$ docker help <CMD>
$ man docker-<CMD>
```

Execute Docker image:

```console
$ docker run hello-world
```

List docker images:

```console
$ docker (image ls|images)
```

List docker containers (runnnig, all, all in quiet mode):

```console
$ docker (container ls|ps)
$ docker (container ls|ps) -a
$ docker (container ls|ps) -aq
```

```console
$ docker build
$ docker run
$ docker tag
```

```console
$ docker start
$ docker restart
$ docker attach
$ docker logs
$ docker top
$ docker stop
$ docker pull
$ docker search
$ docker commit
$ docker history
$ docker port
```

Show logs of container:

```console
$ docker logs <CONTAINER>
```

Follow logs of container with timestamps:

```console
$ docker logs -ft <CONTAINER>
```

```console
$ docker login
$ docker push
```

```console
$ docker inspect
```

```console
$ docker container prune
$ docker container rm $(docker container ls -aq)
$ docker image prune
$ docker image rm $(docker image ls -aq)
```

### Dockerfile

Dockerfile instructions

* RUN
* EXPOSE
* MAINTAINER
* CMD
* ENTRYPOINT
* WORKDIR
* ENV
* USER
* VOLUME
* ADD
* ONBUILD
* STOPSIGNAL

### Compose

### Swarm

```console
$ docker swarm init
$ docker stack deploy
$ docker stack ls
$ docker service ls
$ docker service ps
$ docker stack rm
$ docker swarm leave --force
```

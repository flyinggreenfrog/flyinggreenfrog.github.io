---
title: Docker
last-changed: <time>2018-06-23</time>
knowledgebase: true
categories: [Container, Linux]
---
## Links

* [Docker overview](https://docs.docker.com/engine/docker-overview) <time>2018-06-23</time>
* [Get started with Docker](https://docs.docker.com/get-started) <time>2018-06-23</time>

## Setup

``` sh
# zypper in docker
# systemctl enable docker
# systemctl start docker
```

To avoid prepending `sudo` in front of every `docker` command, add your user to
the `docker` group. Be aware of the security implications as the `docker` group
grants rights equivalent to `root`.

``` sh
# usermod -a -G docker <USER>
```

## Docker

1. Container
2. Services
3. Stack

* `Dockerfile`
* `docker-compose.yml`

## Usage

### Docker

Getting information:

``` sh
$ docker -v|--version
$ docker version
$ docker info
```

Getting help:

``` sh
$ docker <CMD> --help
$ docker help <CMD>
$ man docker-<CMD>
```

``` sh
$ docker run hello-world
```

``` sh
$ docker (image ls|images)
$ docker (container ls|ps)
```

``` sh
$ docker build
$ docker run
$ docker tag
```

``` sh
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

``` sh
$ docker logs <CONTAINER>
```

Follow logs of container with timestamps:

``` sh
$ docker logs -ft <CONTAINER>
```

``` sh
$ docker login
$ docker push
```

``` sh
$ docker inspect
```

``` sh
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

### Swarm

``` sh
$ docker swarm init
$ docker stack deploy
$ docker stack ls
$ docker service ls
$ docker service ps
$ docker stack rm
$ docker swarm leave --force
```

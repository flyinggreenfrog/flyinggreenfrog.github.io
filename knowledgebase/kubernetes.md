---
title: Kubernetes
last-changed: <time>2019-09-01</time>
knowledgebase: true
categories: [Container, Linux]
---
## Links

* [kubernetes: Concepts](https://kubernetes.io/docs/concepts) <time>2019-06-14</time>
* [kubernetes: Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube) <time>2019-06-14</time>
* [kubernetes: Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube) <time>2019-06-14</time>

* [kubernetes: Running Kubernetes Locally via Minikube](https://kubernetes.io/docs/setup/minikube) <time>2018-06-23</time>
* [kubernetes: Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics) <time>2018-06-23</time>
* [helm: Documentation](https://docs.helm.sh) <time>2018-06-23</time>
* [k3s: lightweight kubernetes](https://k3s.io) <time>2019-06-14</time>

## Terms

K8s
: Kubernetes

## Concepts

Basic Objects

* Pod
* Service
* Volume
* Namespace

Controllers

* ReplicaSet
* Deployment
* StatefulSet
* DaemonSet
* Job

## Setup

Kubernetes

```sh
# zypper in kubernetes-master kubernetes-common kubernetes-client
```

Minikube

```sh
# zypper in docker-machine-kvm minikube
```

## Minikube

```sh
$ minikube start --vm-driver kvm
$ ...
$ minikube delete
```

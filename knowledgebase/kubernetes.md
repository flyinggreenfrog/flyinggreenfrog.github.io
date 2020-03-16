---
title: Kubernetes
last-changed: <time>2020-03-16</time>
knowledgebase: true
categories: [Container, Linux]
---
## Links

* [kubernetes: Concepts](https://kubernetes.io/docs/concepts) <time>2020-03-10</time>
* [kubectl](https://kubectl.docs.kubernetes.io) <time>2020-03-11</time>
* [minikube: Getting Started / Linux](https://minikube.sigs.k8s.io/docs/start/linux) <time>2020-03-10</time>
* [kubernetes: Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube) <time>2020-03-10</time>
* [kubernetes: Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics) <time>2020-03-10</time>
* [kubernetes: Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube) <time>2019-06-14</time>
* [kubernetes: Running Kubernetes Locally via Minikube](https://kubernetes.io/docs/setup/minikube) <time>2018-06-23</time>
* [helm: Documentation](https://docs.helm.sh) <time>2018-06-23</time>
* [k3s: lightweight kubernetes](https://k3s.io) <time>2019-06-14</time>

## Terms

K8s
: Kubernetes

PLEG
: Pod Lifecycle Event Generator

## Concepts

Kubernetes Master processes:

* kube-apiserver
* kube-controller-manager
* kube-scheduler
* (cloud-controller-manager: release 1.6)

Non-master nodes / worker (previously minions):

* kubelet
* kube-proxy
* <CONTAINER-RUNTIME>: docker, containerd, CRI-O, rkt

Basic Objects

* Pod
  - One ore more container
  - Shared storage
  - Network
* Service
  - Logical set of pods
  - external traffic
  - load balancing
* Volume
* Namespace

Controllers

* (ReplicationController: historisch)
* ReplicaSet
* Deployment
* StatefulSet
* DaemonSet
* Job
* CronJob

## Setup

Kubectl

```sh
# wget -c -O /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/v1.17.0/bin/linux/amd64/kubectl
# chmod 755 /usr/local/bin/kubectl
```

Minikube

```sh
# zypper in minikube docker-machine-kvm2
```

## Minikube

```sh
$ minikube config set vm-driver kvm
$ minikube start
$ minikube kubectl
$ minikube dashboard
$ minikube start --vm-driver kvm
$ ...
$ minikube delete
```

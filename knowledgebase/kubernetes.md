---
title: Kubernetes
last-changed: <time>2023-02-15</time>
knowledgebase: true
categories: [Container, Linux]
---
## Links

* [kubernetes: Concepts](https://kubernetes.io/docs/concepts) <time>2023-02-01</time>
* [kubernetes: Glossary](https://kubernetes.io/docs/reference/glossary/?fundamental=true) <time>2023-02-01</time>
* [kubernetes: Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics) <time>2023-02-01</time>

* [kubectl](https://kubectl.docs.kubernetes.io) <time>2020-03-11</time>
* [minikube: Getting Started / Linux](https://minikube.sigs.k8s.io/docs/start/linux) <time>2020-03-10</time>
* [kubernetes: Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube) <time>2020-03-10</time>
* [kubernetes: Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube) <time>2019-06-14</time>
* [kubernetes: Running Kubernetes Locally via Minikube](https://kubernetes.io/docs/setup/minikube) <time>2018-06-23</time>
* [helm: Documentation](https://docs.helm.sh) <time>2023-02-14</time>
* [k3s: lightweight kubernetes](https://k3s.io) <time>2019-06-14</time>

* [Operator Framework on Kubernetes](https://cloud.redhat.com/blog/introducing-the-operator-framework) <time>2023-02-15</time>
* [Kubernetes Operator](https://www.redhat.com/de/topics/containers/what-is-a-kubernetes-operator) <time>2023-02-15</time>

## Terms

K8s
: Kubernetes

PLEG
: Pod Lifecycle Event Generator

## Concepts

K8s cluster: at least on worker node

Control plane components:

* kube-apiserver
* etcd
* kube-scheduler
* kube-controller-manager
  - Node controller
  - Job controller
  - EndpointSlice controller
  - ServiceAccount controller
* optional: cloud-controller-manager
  - Node controller
  - Route controller
  - Service controller

Node components (worker):

* kubelet
* kube-proxy
* Container runtime: containerd, CRI-O, rkt, (docker)

Kubernetes object management:

* Imperative commands
* Imperative object configuration
* Declarative object configuration

Basic Objects:

* Pod
  - One ore more container with shared namespaces
  - Shared storage (volumes)
  - Network (IP addresses)
* Service
  - Logical set of pods
  - external traffic
  - load balancing
  - service discovery
* Volume
* Namespace

Controllers:

* Deployment
* (ReplicationController: historisch)
* ReplicaSet
* StatefulSet
* DaemonSet
* Job
* CronJob

Pod:

* Pod phase (status)
* Container states
* Pod conditions


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

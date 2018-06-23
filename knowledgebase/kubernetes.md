---
title: Kubernetes
last-changed: <time>2018-06-23</time>
knowledgebase: true
categories: [Container, Linux]
---
## Links

* [kubernetes: Running Kubernetes Locally via Minikube](https://kubernetes.io/docs/setup/minikube) <time>2018-06-23</time>
* [kubernetes: Hello Minikube](https://kubernetes.io/docs/tutorials/hello-minikube) <time>2018-06-23</time>
* [kubernetes: Kubernetes 101](https://kubernetes.io/docs/tutorials/k8s101) <time>2018-06-23</time>
* [kubernetes: Kubernetes 201](https://kubernetes.io/docs/tutorials/k8s201) <time>2018-06-23</time>
* [helm: Documentation](https://docs.helm.sh) <time>2018-06-23</time>

## Terms

K8s
: Kubernetes

## Setup

Kubernetes

``` sh
# zypper in kubernetes-master kubernetes-common kubernetes-client
```

Minikube

``` sh
# zypper in docker-machine-kvm minikube
```

## Minikube

``` sh
$ minikube start --vm-driver kvm
$ ...
$ minikube delete
```

---
title: 쿠버네티스 - exr10 - DAEMONSETS
author: YHole
date: 2022-04-25 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr10 DAEMONSETS

```bash
# How many DaemonSets are created in the cluster in all namespaces?
# Check all namespaces

kubectl get daemonsets -A

kubectl get daemonsets --all-namespaces
```

```bash
# Which namespace are the DaemonSets created in?

NAMESPACE     NAME              DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   kube-flannel-ds   1         1         1       1            1           <none>                   20m
kube-system   kube-proxy        1         1         1       1            1           kubernetes.io/os=linux   20m

# kube-system
```

```bash
# Which of the below is a DaemonSet?
# 다음 중 어떤 것이 DaemonSet인가

kubectl get all --all-namespaces
# identify the types

or

kubectl get daemonsets.apps --all-namespaces

NAMESPACE     NAME              DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   kube-flannel-ds   1         1         1       1            1           <none>                   18m
kube-system   kube-proxy        1         1         1       1            1           kubernetes.io/os=linux   18m
```

```bash
# On how many nodes are the pods scheduled by the DaemonSet kube-proxy

kubectl describe daemonsets.apps kube-proxy -n kube-system

1
```

```bash
# What is the image used by the POD deployed by the kube-flannel-ds DaemonSet?

kubectl describe daemonset kube-flannel-ds --namespace=kube-system

...
Pod Template:
  Labels:           app=flannel
                    tier=node
  Service Account:  flannel
  Init Containers:
   install-cni:
    Image:      quay.io/coreos/flannel:v0.13.1-rc1 # 이미지
    Port:       <none>
    Host Port:  <none>
    Command:
      cp
    Args:
      -f
      /etc/kube-flannel/cni-conf.json
      /etc/cni/net.d/10-flannel.conflist
    Environment:  <none>
    Mounts:
      /etc/cni/net.d from cni (rw)
      /etc/kube-flannel/ from flannel-cfg (rw)
  Containers:
   kube-flannel:
    Image:      quay.io/coreos/flannel:v0.13.1-rc1
    Port:       <none>
    Host Port:  <none>
...
```

```bash
# Deploy a DaemonSet for FluentD Logging.
# Use the given specifications.

# Name: elasticsearch
# Namespace: kube-system
# Image: k8s.gcr.io/fluentd-elasticsearch:1.20

kubectl create deployment elasticsearch --image=k8s.gcr.io/fluentd-elasticsearch:1.20 -n kube-system --dry-run=client -o yaml > fluentd.yaml

# 맨 밑줄에 추가 (status: {} 밑줄)
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: elasticsearch
  name: elasticsearch
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: elasticsearch
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: elasticsearch
    spec:
      containers:
      - image: k8s.gcr.io/fluentd-elasticsearch:1.20
        name: fluentd-elasticsearch
        resources: {}
status: {}
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  labels:
    app: elasticsearch
  name: elasticsearch
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app: elasticsearch
  template:
    metadata:
      labels:
        app: elasticsearch
    spec:
      containers:
      - image: k8s.gcr.io/fluentd-elasticsearch:1.20
        name: fluentd-elasticsearch

kubectl apply -f fluentd.yaml

# 없을 때
# kubectl create -f fluentd.yaml

# 교체 할때
# kubectl replace -f fluentd.yaml
```

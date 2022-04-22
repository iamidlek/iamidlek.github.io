---
title: 쿠버네티스 - exr7 - TAINTS AND TOLERATIONS
author: YHole
date: 2022-04-22 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr7 TAINTS AND TOLERATIONS

```bash
# How many nodes exist on the system?
# Including the controlplane node.

kubectl get node
```

```bash
# Do any taints exist on node01 node?

kubectl describe node node01
# kubectl describe node node01 | grep -i taints

Name:               node01
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=node01
                    kubernetes.io/os=linux
Annotations:        flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"6e:d7:be:b4:42:2c"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 10.10.37.12
                    kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Wed, 20 Apr 2022 05:51:58 +0000
Taints:             <none> # Taints 없음
...
```

```bash
# Create a taint on node01 with key of spray, value of mortein and effect of NoSchedule

Key = spray
Value = mortein
Effect = NoSchedule

kubectl taint nodes node01 spray=mortein:NoSchedule
```

```bash
# Create a new pod with the nginx image and pod name as mosquito.

kubectl run mosquito --image=nginx
```

```bash
# What is the state of the POD?

kubectl describe pod mosquito

# Why do you think the pod is in a pending state?

Name:         mosquito
Namespace:    default
Priority:     0
Node:         <none>

nodes are available: 1 node(s) had taint {node-role.kubernetes.io/master: }, that the pod didn't tolerate, 1 node(s) had taint {spray: mortein}, that the pod didn't tolerate.
```

```bash
# Create another pod named bee with the nginx image, which has a toleration set to the taint mortein.

apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - image: nginx
    name: bee # pod이름과 일치하지 않아도 상관은 없음
  tolerations:
  - key: spray
    value: mortein
    effect: NoSchedule
    operator: Equal # 기본값 Equal => value필요


```

```bash
# Do you see any taints on controlplane node?

kubectl describe node controlplane | grep -i taints

Taints: node-role.kubernetes.io/master:NoSchedule

# Yes,NoSchedule
```

```bash
# Remove the taint on controlplane, which currently has the taint effect of NoSchedule.

# - 를 마지막에 붙여주면 untainted
kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-
```

```bash
# What is the state of the pod mosquito now?

kubectl get pod mosquito

Running
```

```bash
# Which node is the POD mosquito on now?

kubectl get pod mosquito -o wide

NAME       READY   STATUS    RESTARTS   AGE   IP           NODE           NOMINATED NODE   READINESS GATES
mosquito   1/1     Running   0          14m   10.244.0.4   controlplane   <none>           <none>

controlplane
```

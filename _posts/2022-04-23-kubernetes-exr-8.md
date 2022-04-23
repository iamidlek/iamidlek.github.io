---
title: 쿠버네티스 - exr8 - NODE AFFINITY
author: YHole
date: 2022-04-23 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr8 NODE AFFINITY

```bash
# How many Labels exist on node node01?

kubectl describe node node01

Name:               node01
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=node01
                    kubernetes.io/os=linux
```

```bash
# What is the value set to the label beta.kubernetes.io/arch on node01?

kubectl get node node01 --show-labels
```

```bash
# Apply a label color=blue to node node01

kubectl label node node01 color=blue
```

```bash
# Create a new deployment named blue with the nginx image and 3 replicas.

kubectl create deployment blue --image=nginx --replicas=3
```

```bash
# Which nodes can the pods for the blue deployment be placed on?
# Make sure to check taints on both nodes!

kubectl describe node controlplane | grep -i taints
# Taints:             <none>
kubectl describe node node01 | grep -i taints
# Taints:             <none>

# 양쪽 다 생성가능
```

```bash
# Set Node Affinity to the deployment to place the pods on node01 only.

# Name: blue
# Replicas: 3
# Image: nginx
# NodeAffinity:
# requiredDuringSchedulingIgnoredDuringExecution
# Key: color
# values: blue

apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue

kubectl delete deployment blue

kubectl apply -f deploy.yaml
```

```bash
# Which nodes are the pods placed on now?

kubectl get pods -o wide
NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
blue-77978c84c6-7gcrx   1/1     Running   0          2m32s   10.244.1.6   node01   <none>           <none>
blue-77978c84c6-clwpt   1/1     Running   0          2m32s   10.244.1.7   node01   <none>           <none>
blue-77978c84c6-pr9ps   1/1     Running   0          2m33s   10.244.1.5   node01   <none>           <none>
```

```bash
# Create a new deployment named red with the nginx image and 2 replicas, and ensure it gets placed on the controlplane node only.
# Use the label - node-role.kubernetes.io/master - set on the controlplane node.

# 노드의 라벨
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=controlplane
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node-role.kubernetes.io/master=


apiVersion: apps/v1
kind: Deployment
metadata:
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists

kubectl apply -f sample.yaml
```

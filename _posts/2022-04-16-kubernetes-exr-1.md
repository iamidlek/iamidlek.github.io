---
title: 쿠버네티스 - exr1 - POD
author: YHole
date: 2022-04-16 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr1 pod

```bash
# How many pods exist on the system?
kubectl get pods
```

```bash
# Create a new pod with the nginx image.
kubectl run nginx --image=nginx
```

```bash
# What is the image used to create the new pods?
# You must look at one of the new pods in detail to figure this out.
kubectl describe pod <pod-name>
```

```bash
# Which nodes are these pods placed on?
kubectl get pods -o wide

or

kubectl describe pod <pod-name>
```

```bash
# How many containers are part of the pod webapp?

kubectl describe pod webapp

Name:         webapp
Namespace:    default
Priority:     0
Node:         controlplane/172.25.0.77
Start Time:   Sat, 16 Apr 2022 12:53:34 +0000
Labels:       <none>
Annotations:  <none>
Status:       Pending
IP:           10.42.0.13
IPs:
  IP:  10.42.0.13
Containers:
# here
```

```bash
# What images are used in the new webapp pod?
# You must look at all the pods in detail to figure this out.

kubectl describe pod webapp
```

```bash
# What is the state of the container agentx in the pod webapp?
# Wait for it to finish the ContainerCreating state

kubectl describe pod webapp
# waitting
agentx:
    Container ID:
    Image:          agentx
    Image ID:
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ErrImagePull
    Ready:          False
```

```bash
# Why do you think the container agentx in pod webapp is in error?

Reason:  ErrImagePull
# A Docker image with this name doesn't exist on Docker Hub
```

```bash
# What does the READY column in the output of the kubectl get pods command indicate?

# Running Containers in POD/Total Containers in POD
```

```bash
# Delete the webapp Pod.
# Once deleted, wait for the pod to fully terminate.

kubectl delete pod webapp
```

```bash
# Create a new pod with the name redis and with the image redis123.
# Use a pod-definition YAML file. And yes the image name is wrong!

# redis => name,run=redis
# --dry-run=client => not run
kubectl run redis --image=redis123 --dry-run=client -o yaml > pod-definition.yaml

kubectl create -f pod-definition.yaml

kubectl get pods
# redis           0/1     ErrImagePull   0            8s

kubectl describe pod redis
#
Name:         redis
Namespace:    default
Priority:     0
Node:         controlplane/172.25.0.77
Start Time:   Sat, 16 Apr 2022 13:05:21 +0000
Labels:       run=redis
Annotations:  <none>
Status:       Pending
IP:           10.42.0.14
IPs:
  IP:  10.42.0.14
Containers:
  redis:
    Container ID:
    Image:          redis123
    Image ID:
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
```

```bash
# Now change the image on this pod to redis.
# Once done, the pod should be in a running state.

kubectl edit pod redis
# change image redis123 => redis

or

vi pod-definition.yaml
# spec:
#   containers:
#   - image: redis     <= change
kubectl apply -f pod-definition.yaml
# 변경된 파일 사항을 Pod에 적용
# create 를 대신하여 사용해도 pod는 생성된다.
```

| command | 기존 리소스 없음     | 기존 리소스가 존재             |
| ------- | -------------------- | ------------------------------ |
| create  | 새로운 리소스가 생성 | ERROR가 발생                   |
| apply   | 새로운 리소스가 생성 | 부분적인 spec을 적용           |
| replace | ERROR가 발생         | 리소스가 삭제된 뒤 새롭게 생성 |

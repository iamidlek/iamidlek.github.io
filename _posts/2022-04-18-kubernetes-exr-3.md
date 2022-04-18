---
title: 쿠버네티스 - exr3 - Deployments
author: YHole
date: 2022-04-18 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr3 Deployments

```bash
# default namespace
# How many Deployments exist on the system?
# In the current(default) namespace.

k get pods
k get rs

k get ns

NAME              STATUS   AGE
default           Active   7m55s
kube-system       Active   7m54s
kube-public       Active   7m54s
kube-node-lease   Active   7m54s

k get deployments
```

```bash
# How many Deployments exist on the system now?
# We just created a Deployment! Check again!

k get deployments
```

```bash
# How many ReplicaSets exist on the system now?

k get rs
```

```bash
# Out of all the existing PODs, how many are ready?

k get po
```

```bash
# What is the image used to create the pods in the new deployment?

k describe deployments frontend-deployment

...
Containers:
   busybox-container:
    Image:      busybox888
    Port:       <none>
    Host Port:  <none>
    ...
```

```bash
# Create a new Deployment using the deployment-definition-1.yaml file located at /root/.
# There is an issue with the file, so try to fix it.

k create -f deployment-definition-1.yaml

Error from server (BadRequest): error when creating "deployment-definition-1.yaml": deployment in version "v1" cannot be handled as a Deployment: no kind "deployment" is registered for version "apps/v1" in scheme "k8s.io/apimachinery@v1.23.3-k3s1/pkg/runtime/scheme.go:100"

kubectl explain deployment | head -n1
# 설명 대문자 D로 시작함

vi deployment-definition-1.yaml
apiVersion: apps/v1
kind: Deployment
```

```bash
# Create a new Deployment with the below attributes using your own deployment definition file.

# Name: httpd-frontend;
# Replicas: 3;
# Image: httpd:2.4-alpine

touch temp.yaml
vi temp.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd-container
        image: httpd:2.4-alpine

k create -f temp.yaml
```

---
title: 쿠버네티스 - exr17 - ENV VARIABLES
author: YHole
date: 2022-05-02 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr17 ENV VARIABLES

```bash
# How many PODs exist on the system?
# in the current(default) namespace

k get pods
```

```bash
# What is the environment variable name set on the container in the pod?

k describe pod webapp-color

Containers:
  webapp-color:
    Container ID:   containerd://70e3d79ced9c4d76cdd211a51539d482f5d85548b50051a15da6d97744d7637f
    Image:          kodekloud/webapp-color
    Image ID:       docker.io/kodekloud/webapp-color@sha256:99c3821ea49b89c7a22d3eebab5c2e1ec651452e7675af243485034a72eb1423
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sat, 30 Apr 2022 09:31:38 +0000
    Ready:          True
    Restart Count:  0
    Environment: # 환경 변수
      APP_COLOR:  pink
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-h9sf6 (ro)
```

```bash
# What is the value set on the environment variable APP_COLOR on the container in the pod?

APP_COLOR:  pink

# pink
```

```bash
# Update the environment variable on the POD to display a green background
# Note: Delete and recreate the POD. Only make the necessary changes. Do not modify the name of the Pod.

Pod Name: webapp-color
Label Name: webapp-color
Env: APP_COLOR=green


k edit pod webapp-color
# error: pods "webapp-color" is invalid
# A copy of your changes has been stored to "/tmp/kubectl-edit-3765636405.yaml"

k delete pod webapp-color

k create -f /tmp/kubectl-edit-3765636405.yaml

or

apiVersion: v1
kind: Pod
metadata:
  labels:
    name: webapp-color
  name: webapp-color
  namespace: default
spec:
  containers:
  - env:
    - name: APP_COLOR
      value: green
    image: kodekloud/webapp-color
    name: webapp-color
```

```bash
# How many ConfigMaps exists in the default namespace?

k get configmaps

NAME               DATA   AGE
kube-root-ca.crt   1      20m
db-config          3      21s
```

```bash
# Identify the database host from the config map db-config

 k describe configmaps db-config
Name:         db-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
DB_HOST:
----
SQL01.example.com # 호스트명
DB_NAME:
----
SQL01
DB_PORT:
----
3306

BinaryData
====

Events:  <none>
```

```bash
# Create a new ConfigMap for the webapp-color POD. Use the spec given below.

ConfigName Name: webapp-config-map
Data: APP_COLOR=darkblue

k create configmap webapp-config-map --from-literal=APP_COLOR=darkblue
```

```bash
# Update the environment variable on the POD to use the newly created ConfigMap
# Note: Delete and recreate the POD. Only make the necessary changes. Do not modify the name of the Pod.

Pod Name: webapp-color
EnvFrom: webapp-config-map

k delete pod webapp-color

#  yaml 작성
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: webapp-color
  name: webapp-color
  namespace: default
spec:
  containers:
  - envFrom:
    - configMapRef:
         name: webapp-config-map
    image: kodekloud/webapp-color
    name: webapp-color

k create -f # 경로
```

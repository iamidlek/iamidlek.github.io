---
title: 쿠버네티스 - exr16 - Commands and Arguments
author: YHole
date: 2022-05-01 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr16 Commands and Arguments

```bash
# How many PODs exist on the system?
# in the current(default) namespace

k get pods

NAME             READY   STATUS    RESTARTS   AGE
ubuntu-sleeper   1/1     Running   0          26s
```

```bash
# What is the command used to run the pod ubuntu-sleeper?

k describe pod ubuntu-sleeper

Name:         ubuntu-sleeper
Namespace:    default
Priority:     0
Node:         controlplane/172.25.0.62
Start Time:   Tue, 26 Apr 2022 06:16:58 +0000
Labels:       <none>
Annotations:  <none>
Status:       Running
IP:           10.42.0.9
IPs:
  IP:  10.42.0.9
Containers:
  ubuntu:
    Container ID:  containerd://7ce922d93876f219daf07d3878865939689ff64f5490d1c1905e26cac2d823c8
    Image:         ubuntu
    Image ID:      docker.io/library/ubuntu@sha256:2a7dffab37165e8b4f206f61cfd984f8bb279843b070217f6ad310c9c31c9c7c
    Port:          <none>
    Host Port:     <none>
    Command:  # 명령어 확인
      sleep
      4800
    State:          Running
```

```bash
# Create a pod with the ubuntu image to run a container to sleep for 5000 seconds. Modify the file ubuntu-sleeper-2.yaml.

# Note: Only make the necessary changes. Do not modify the name.

vi ubuntu-sleeper-2.yaml

apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-2
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command: ['sleep','5000']

k apply -f ubuntu-sleeper-2.yaml
```

```bash
# Create a pod using the file named ubuntu-sleeper-3.yaml. There is something wrong with it. Try to fix it!

# Note: Only make the necessary changes. Do not modify the name.

Pod Name: ubuntu-sleeper-3
Command: sleep 1200

piVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-3
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:
      - "sleep"
      - 1200 # "1200"
```

```bash
# Update pod ubuntu-sleeper-3 to sleep for 2000 seconds.

# Note: Only make the necessary changes. Do not modify the name of the pod. Delete and recreate the pod if necessary.

apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-3
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:
      - "sleep"
      - "2000"

k delete pod ubuntu-sleeper-3

k apply -f ubuntu-sleeper-3.yaml
```

```bash
# Inspect the file Dockerfile given at /root/webapp-color directory.
# What command is run at container startup?

vi webapp-color/Dockerfile

---------------------
ROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]
---------------------
```

```bash
# Inspect the file Dockerfile2 given at /root/webapp-color directory.
# What command is run at container startup?

ROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]

CMD ["--color", "red"]

# "python", "app.py", "--color", "red"
```

```bash
# Inspect the two files under directory webapp-color-2. What command is run at container startup?

# Assume the image was created from the Dockerfile in this folder.

webapp-color-2/webapp-color-pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: webapp-green
  labels:
      name: webapp-green
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    command: ["--color","green"]

# "--color","green"
```

```bash
# Inspect the two files under directory webapp-color-3. What command is run at container startup?

# Assume the image was created from the Dockerfile in this folder.

apiVersion: v1
kind: Pod
metadata:
  name: webapp-green
  labels:
      name: webapp-green
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    command: ["python", "app.py"]
    args: ["--color", "pink"]

# "python", "app.py", "--color", "pink"
```

```bash
# Create a pod with the given specifications.
# By default it displays a blue background.
# Set the given command line arguments to change it to green

Pod Name: webapp-green
Image: kodekloud/webapp-color
Command line arguments: --color=green


apiVersion: v1
kind: Pod
metadata:
  name: webapp-green
  labels:
      name: webapp-green
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    args: ["--color", "green"]

k apply -f sample.yaml
```

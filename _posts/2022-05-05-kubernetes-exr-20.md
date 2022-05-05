---
title: 쿠버네티스 - exr20 - INIT CONTAINERS
author: YHole
date: 2022-05-05 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr20 INIT CONTAINERS

```bash
# Identify the pod that has an initContainer configured.
kubectl describe pod

# blue에 Init Containers 정의가 있다
Name:         blue
Namespace:    default
Priority:     0
Node:         controlplane/172.25.0.26
Start Time:   Sat, 30 Apr 2022 13:06:57 +0000
Labels:       <none>
Annotations:  <none>
Status:       Running
IP:           10.42.0.11
IPs:
  IP:  10.42.0.11
Init Containers:
  init-myservice:
    Container ID:  containerd://7b09b0e8b8236745cccbf42ff98508bd525e623c393a7fe4a9142b95e3b7da5c
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:d2b53584f580310186df7a2055ce3ff83cc0df6caacf1e3489bff8cf5d0af5d8
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      sleep 5
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Sat, 30 Apr 2022 13:07:01 +0000
      Finished:     Sat, 30 Apr 2022 13:07:06 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-jfbmr (ro)
Containers:
  green-container-1:
```

```bash
# What is the image used by the initContainer on the blue pod?

Init Containers:
  init-myservice:
    Container ID:  containerd://7b09b0e8b8236745cccbf42ff98508bd525e623c393a7fe4a9142b95e3b7da5c
    Image:         busybox
```

```bash
# What is the state of the initContainer on pod blue
# Why is the initContainer terminated? What is the reason?

    State:          Terminated
      Reason:       Completed
```

```bash
# We just created a new app named purple. How many initContainers does it have?

kubectl describe pod purple

Init Containers:
  warm-up-1:
    Container ID:  containerd://791f6f5a38670f155e4affec3d5279da0cc02834378308aadcb1da9b2d6b3603
    Image:         busybox:1.28
    Image ID:      docker.io/library/busybox@sha256:141c253bc4c3fd0a201d32dc1f493bcf3fff003b6df416dea4f41046e0f37d47
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      sleep 600
    State:          Running
      Started:      Sat, 30 Apr 2022 13:17:52 +0000
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9pzqm (ro)
  warm-up-2:
    Container ID:
    Image:         busybox:1.28
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      sleep 1200
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-9pzqm (ro)
```

```bash
# What is the state of the POD?

Name:         purple
Namespace:    default
Priority:     0
Node:         controlplane/172.25.0.26
Start Time:   Sat, 30 Apr 2022 13:17:51 +0000
Labels:       <none>
Annotations:  <none>
Status:       Pending
```

```bash
# How long after the creation of the POD will the application come up and be available to users?

warm-up-1 sleep 600 초
warm-up-2 sleep 1200 초

즉 30분 후
```

```bash
# Update the pod red to use an initContainer that uses the busybox image and sleeps for 20 seconds
# Delete and re-create the pod if necessary. But make sure no other configurations change.

Pod: red
initContainer Configured Correctly

k delete pods red

apiVersion: v1
kind: Pod
metadata:
  name: red
  namespace: default
spec:
  containers:
  - command:
    - sh
    - -c
    - echo The app is running! && sleep 3600
    image: busybox:1.28
    name: red-container
  initContainers:
  - image: busybox
    name: red-initcontainer
    command:
      - "sleep"
      - "20"

k create -f sample.yaml
```

```bash
# A new application orange is deployed. There is something wrong with it. Identify and fix the issue.

# Once fixed, wait for the application to run before checking solution.

k describe pod orange

Init Containers:
  init-myservice:
    Container ID:  containerd://839654fb1255535aa09033ebb395bdf879c00b43d8c2d4205a9b90ba7d479524
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:d2b53584f580310186df7a2055ce3ff83cc0df6caacf1e3489bff8cf5d0af5d8
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      sleeeep 2; # 명령어 오타

k get pod orange -o yaml > /root/orange.yaml

k delete pod orange

vi orange.yaml

k create -f orange.yaml
```

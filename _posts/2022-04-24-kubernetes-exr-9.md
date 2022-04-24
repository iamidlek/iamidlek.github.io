---
title: 쿠버네티스 - exr9 - RESOURCE LIMITS
author: YHole
date: 2022-04-24 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr9 RESOURCE LIMITS

```bash
# A pod called rabbit is deployed. Identify the CPU requirements set on the Pod
# in the current(default) namespace

k describe pod rabbit

Limits:
  cpu:  2
Requests:
  cpu:  1 # 1만큼 요청
```

```bash
# Delete the rabbit Pod.
# Once deleted, wait for the pod to fully terminate.

k delete pod rabbit
```

```bash
# Another pod called elephant has been deployed in the default namespace. It fails to get to a running state. Inspect this pod and identify the Reason why it is not running.

k describe pod elephant

...
  State:        Waiting
    Reason:     CrashLoopBackOff
  Last State:   Terminated
    Reason:     OOMKilled # 이유에 해당
...

The status OOMKilled indicates that it is failing because the pod ran out of memory. Identify the memory limit set on the POD.
```

```bash
# The elephant pod runs a process that consume 15Mi of memory. Increase the limit of the elephant pod to 20Mi.
# Delete and recreate the pod if required. Do not modify anything other than the required fields.

# k get po elephant -o yaml > elephant.yaml
# 새로운 파일을 만들어서 작성해야 동작하는 것 같다

apiVersion: v1
kind: Pod
metadata:
  name: elephant
  namespace: default
spec:
  containers:
  - args:
    - --vm
    - "1"
    - --vm-bytes
    - 15M
    - --vm-hang
    - "1"
    command:
    - stress
    image: polinux/stress
    name: mem-stress
    resources:
      limits:
        memory: 20Mi
      requests:
        memory: 5Mi

k replace -f elephant.yaml --force
```

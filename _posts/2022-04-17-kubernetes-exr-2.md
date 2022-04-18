---
title: 쿠버네티스 - exr2 - REPLICASETS
author: YHole
date: 2022-04-17 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr2 REPLICASETS

```bash
# How many ReplicaSets exist on the system?
kubectl get replicaset
kubectl get rs

# one ReplicaSet
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       10s
```

```bash
# How many PODs are DESIRED in the new-replica-set?
4
```

```bash
# What is the image used to create the pods in the new-replica-set?
kubectl describe replicaset

Name:         new-replica-set
Namespace:    default
Selector:     name=busybox-pod
Labels:       <none>
Annotations:  <none>
Replicas:     4 current / 4 desired
Pods Status:  0 Running / 4 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  name=busybox-pod
  Containers:
   busybox-container:
    Image:      busybox777
```

```bash
# How many PODs are READY in the new-replica-set?

4 Waiting
# 정답은 READY된 POD는 0
0
```

```bash
# Why do you think the PODs are not ready?

kubectl describe pods

# 확인

Containers:
  busybox-container:
    Container ID:
    Image:         busybox777
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      echo Hello Kubernetes! && sleep 3600
    State:          Waiting
      Reason:       ImagePullBackOff
...

Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  4m33s                  default-scheduler  Successfully assigned default/new-replica-set-7l9hw to controlplane
  Normal   Pulling    2m58s (x4 over 4m31s)  kubelet            Pulling image "busybox777"
  Warning  Failed     2m58s (x4 over 4m31s)  kubelet            Failed to pull image "busybox777": rpc error: code = Unknown desc = failed to pull and unpack image "docker.io/library/busybox777:latest": failed to resolve reference "docker.io/library/busybox777:latest": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
  Warning  Failed     2m58s (x4 over 4m31s)  kubelet            Error: ErrImagePull
  Warning  Failed     2m44s (x6 over 4m31s)  kubelet            Error: ImagePullBackOff
  Normal   BackOff    2m32s (x7 over 4m31s)  kubelet            Back-off pulling image "busybox777"
# 이미지 가져오기 실패
```

```bash
# Delete any one of the 4 PODs.

kubectl get pods
kubectl delete pod new-replica-set-vz6wz
```

```bash
# How many PODs exist now?
4

# repllicaset에 의해 바로 하나의 pod가 생성되기 때문
```

```bash
# Create a ReplicaSet using the replicaset-definition-1.yaml file located at /root/.

# There is an issue with the file, so try to fix it.

kubectl create -f replicaset-definition-1.yaml
# error: unable to recognize "replicaset-definition-1.yaml": no matches for kind "ReplicaSet" in version "v1"

kubectl explain replicaset | grep VERSION
# VERSION:  apps/v1

vi replicaset-definition-1.yaml # 버전 정보 수정
kubectl create -f replicaset-definition-1.yaml

kubectl get rs
```

```bash
# Fix the issue in the replicaset-definition-2.yaml file and create a ReplicaSet using it.

kubectl create -f replicaset-definition-2.yaml
# The ReplicaSet "replicaset-2" is invalid: spec.template.metadata.labels: Invalid value: map[string]string{"tier":"nginx"}: `selector` does not match template `labels`

vi replicaset-definition-2.yaml

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend # nginx로 맞춰주어야 함
  template:
    metadata:
      labels:
        tier: nginx
    spec:
      containers:
      - name: nginx
        image: nginx

kubectl create -f replicaset-definition-2.yaml
```

```bash
# Delete the two newly created ReplicaSets - replicaset-1 and replicaset-2

kubectl delete replicaset <replicaset-name>

or

kubectl delete -f <file-name>.yaml
```

```bash
# Fix the original replica set new-replica-set to use the correct busybox image.

# Either delete and recreate the ReplicaSet or Update the existing ReplicaSet and then delete all PODs, so new ones with the correct image will be created.

NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       16m

kubectl edit replicaset new-replica-set
# 스펙 내용의 image이름 변경 후 저장

# pod들을 지워야 변경된 pod로 생성됨
kubectl delete pod new-replica-set-7l9hw new-replica-set-9r6pr new-replica-set-dngnz new-replica-set-qv5xq
```

```bash
# Scale the ReplicaSet to 5 PODs.

# 1
kubectl edit replicaset new-replica-set
# 스펙 변경하기 => 바로 적용됨

or

# 2
kubectl scale rs new-replica-set --replicas=5
# 스펙 변경하기 => 바로 적용됨
```

```bash
# Now scale the ReplicaSet down to 2 PODs.
kubectl scale rs new-replica-set --replicas=2
```

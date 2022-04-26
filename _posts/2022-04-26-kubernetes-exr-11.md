---
title: 쿠버네티스 - exr11 - Static pods
author: YHole
date: 2022-04-26 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr11 Static pods

```bash
# How many static pods exist in this cluster in all namespaces?

kubectl get pod --all-namespaces

NAMESPACE     NAME                                   READY   STATUS    RESTARTS   AGE
ube-system   coredns-74ff55c5b-7ssrv                1/1     Running   0          20m
kube-system   coredns-74ff55c5b-9gfnq                1/1     Running   0          20m
kube-system   etcd-controlplane                      1/1     Running   0          20m
kube-system   kube-apiserver-controlplane            1/1     Running   0          20m
kube-system   kube-controller-manager-controlplane   1/1     Running   0          20m
kube-system   kube-flannel-ds-5sfjc                  1/1     Running   0          20m
kube-system   kube-flannel-ds-kf5ng                  1/1     Running   0          20m
kube-system   kube-proxy-gsz9q                       1/1     Running   0          20m
kube-system   kube-proxy-l9gm8                       1/1     Running   0          20m
kube-system   kube-scheduler-controlplane            1/1     Running   0          20m

# -controlplane 부분
4
```

```bash
# Which of the below components is NOT deployed as a static pod?

kube-apiserver
kube-controller-manager
etcd
coredns

# coredns
```

```bash
# Which of the below components is NOT deployed as a static POD?

kube-apiserver
kube-controller-manager
kube-proxy
kube-scheduler

# kube-proxy is deployed as a DaemonSet and hence, it is not a static pod.
# 뒤에 -controlplane 이 붙지 않은 pod는 static pod가 아님
```

```bash
# On which nodes are the static pods created currently?

kubectl get pod --all-namespaces -o wide

# static pods가 어느 노드에 만들어졌는지 확인
# controlplane
```

```bash
# What is the path of the directory holding the static pod definition files?

# 프로세스
# -a 데몬 프로세스처럼 너미널에 종속되지 않은 모든 프로세스를 출력
# 프로세스의 소유자를 기준으로 출력한다. (ps ax만 하면 USER 기준의 정보가 안뜸, 따라서 aux 이렇게 같이 대게 써준다)
# 데몬 프로세스처럼 터미널에 종속되지 않는 프로세스를 출력한다. 보통 a옵션과 결합하여 모든 프로세스를 출력할 때 사용한다.
ps -aux | grep kubelet

# 해당 하는 이하 부분 찾기
--config=/var/lib/kubelet/config.yaml

# -i : 대/소문자 무시
grep -i staticpod /var/lib/kubelet/config.yaml

# 위치
# /etc/kubernetes/manifests
```

```bash
# How many pod definition files are present in the manifests folder?

ls /etc/kubernetes/manifests/
```

```bash
# What is the docker image used to deploy the kube-api server as a static pod?

kubectl describe pod kube-api -n kube-system

Name:                 kube-apiserver-controlplane
Namespace:            kube-system
...
Containers:
  kube-apiserver:
    Container ID:  docker://0dbd6c7ea5b5e856d95f248b6802c362ade4f3e58bd69625f9d7127ecc421a8b
    Image:         k8s.gcr.io/kube-apiserver:v1.20.0
```

```bash
# Create a static pod named static-busybox that uses
# the busybox image and the command sleep 1000

# Name: static-busybox
# Image: busybox

# /etc/kubernetes/manifests 해당 경로에 static-busybox.yaml을 만들면 끝
touch /etc/kubernetes/manifests/static-busybox.yaml

or

kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml

apiVersion: v1
kind: Pod
metadata:
  labels:
    run: static-busybox
  name: static-busybox
spec:
  containers:
  - command:
    - sleep
    - "1000"
    image: busybox
    name: static-busybox
  restartPolicy: Never
```

```bash
# Edit the image on the static pod to use busybox:1.28.4

kubectl run --restart=Never --image=busybox:1.28.4 static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml

# 해당 명령어로 파일 자체를 바꾸거나

apiVersion: v1
kind: Pod
metadata:
  labels:
    run: static-busybox
  name: static-busybox
spec:
  containers:
  - command:
    - sleep
    - "1000"
    image: busybox:1.28.4
    name: static-busybox
  restartPolicy: Never

# 이미지 버전 지정
```

```bash
# We just created a new static pod named static-greenbox. Find it and delete it.

kubectl get pods --all-namespaces -o wide

NAMESPACE     NAME                                   READY   STATUS    RESTARTS   AGE     IP           NODE           NOMINATED NODE   READINESS GATES
default       static-busybox-controlplane            1/1     Running   0          8m33s   10.244.0.6   controlplane   <none>           <none>
default       static-greenbox-node01                 1/1     Running   0          5m56s   10.244.1.2   node01         <none>           <none>
...

# node1에 있으므로 node1으로 이동
ssh node01

# ps -aux | grep kubelet    or
# 커널 프로세스를 제외한 모든 프로세스를 출력해준다
ps -ef |  grep /usr/bin/kubelet
# --config=/var/lib/kubelet/config.yaml 찾기

grep -i staticpod /var/lib/kubelet/config.yaml
# /etc/just-to-mess-with-you 경로 확인

ls /etc/just-to-mess-with-you

rm /etc/just-to-mess-with-you/greenbox.yaml

# CTRL + D or type exit
# You should return to the controlplane node.

# 지워진 상태 확인
kubectl get pods --all-namespaces -o wide  | grep static-greenbox
```

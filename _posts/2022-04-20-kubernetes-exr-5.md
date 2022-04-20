---
title: 쿠버네티스 - exr5 - MANUAL SCHEDULING
author: YHole
date: 2022-04-20 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr5 MANUAL SCHEDULING

```bash
# A pod definition file nginx.yaml is given. Create a pod using the file.
# Only create the POD for now. We will inspect its status next.

kubectl create -f nginx.yaml
```

```bash
# What is the status of the created POD?

kubectl describe pod nginx
Name:         nginx
Namespace:    default
Priority:     0
Node:         <none>
Labels:       <none>
Annotations:  <none>
Status:       Pending # 현재 상태
IP:
IPs:          <none>
Containers:
  nginx:
    Image:        nginx
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-ngnfv (ro)
Volumes:
  default-token-ngnfv:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-ngnfv
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:          <none>
```

```bash
# Why is the POD in a pending state?
# Inspect the environment for various kubernetes control plane components.

Run the command: kubectl get pods --namespace kube-system to see the status of scheduler pod. We have removed the scheduler from this Kubernetes cluster. As a result, as it stands, the pod will remain in a pending state forever.

kubectl get pods --namespace kube-system
# 스케쥴러가 존재하지 않음
NAME                                   READY   STATUS    RESTARTS   AGE
coredns-74ff55c5b-22r5s                1/1     Running   0          9m18s
coredns-74ff55c5b-dxkgt                1/1     Running   0          9m17s
etcd-controlplane                      1/1     Running   0          9m25s
kube-apiserver-controlplane            1/1     Running   0          9m25s
kube-controller-manager-controlplane   1/1     Running   0          9m25s
kube-flannel-ds-69zfp                  1/1     Running   0          9m18s
kube-flannel-ds-wsntz                  1/1     Running   0          8m59s
kube-proxy-f4fvc                       1/1     Running   0          8m59s
kube-proxy-kqzmb                       1/1     Running   0          9m18s
```

```bash
# Manually schedule the pod on node01.
# Delete and recreate the POD if necessary.

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01

# nodeName을 특정해 준다
kubectl create -f nginx.yaml
```

```bash
# Now schedule the same pod on the controlplane node.
# Delete and recreate the POD if necessary.

Status: Running
Pod: nginx
Node: controlplane?

kubectl get pod -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          5m34s   10.244.1.2   node01   <none>           <none>

kubectl delete pod nginx

# vi 파일에서 nodeName을 controlplane로 변경

kubectl create -f nginx.yaml
```

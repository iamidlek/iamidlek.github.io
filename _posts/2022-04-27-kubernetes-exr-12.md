---
title: 쿠버네티스 - exr12 - Multiple Schedulers
author: YHole
date: 2022-04-27 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr12 Multiple Schedulers

```bash
# What is the name of the POD that deploys the default kubernetes scheduler in this environment?

k get pods -n kube-system

kube-scheduler-controlplane
```

```bash
# What is the image used to deploy the kubernetes scheduler?
# Inspect the kubernetes scheduler pod and identify the image

kubectl describe pod kube-scheduler-controlplane -n kube-system

...
Containers:
  kube-scheduler:
    Container ID:  docker://98ecf5b07fda8d18afcf60964413960e4f95e0387d8919a4cc7e552ded601735
    Image:         k8s.gcr.io/kube-scheduler:v1.23.0
    Image ID:      docker-pullable://k8s.gcr.io/kube-scheduler@sha256:af8166ce28baa7cb902a2c0d16da865d5d7c892fe1b41187fd4be78ec6291c23
    Port:          <none>
    Host Port:     <none>ㅌ
```

```bash
# We have already created the ServiceAccount and ClusterRoleBinding that our custom scheduler will make use of.

# Checkout the following kubernetes objects:

# ServiceAccount: my-scheduler (kube-system namespace)
# ClusterRoleBinding: my-scheduler-as-kube-scheduler
# ClusterRoleBinding: my-scheduler-as-volume-scheduler

kubectl get sa -n kube-system | grep my-scheduler


--------
--------
# Let's create a configmap that the new scheduler will employ using the concept of ConfigMap as a volume.
# Create a configmap with name my-scheduler-config using the content of file /root/my-scheduler-config.yaml

kubectl get configmaps -n kube-system

kubectl create -n kube-system configmap my-scheduler-config --from-file=/root/my-scheduler-config.yaml

# my-scheduler-config.yaml
apiVersion: kubescheduler.config.k8s.io/v1beta2
kind: KubeSchedulerConfiguration
profiles:
  - schedulerName: my-scheduler
leaderElection:
  leaderElect: false
```

```bash
# Deploy an additional scheduler to the cluster following the given specification.
# Use the manifest file provided at /root/my-scheduler.yaml. Use the same image as used by the default kubernetes scheduler.

Name: my-scheduler
Status: Running
Correct image used?

apiVersion: v1
kind: Pod
metadata:
  labels:
    run: my-scheduler
  name: my-scheduler
  namespace: kube-system
spec:
  serviceAccountName: my-scheduler
  containers:
  - command:
    - /usr/local/bin/kube-scheduler
    - --config=/etc/kubernetes/my-scheduler/my-scheduler-config.yaml
    image: k8s.gcr.io/kube-scheduler:v1.23.0 # 이미지 수정
    livenessProbe:
      httpGet:
        path: /healthz
        port: 10259
        scheme: HTTPS
      initialDelaySeconds: 15
    name: kube-second-scheduler
    readinessProbe:
      httpGet:
        path: /healthz
        port: 10259
        scheme: HTTPS
    resources:
      requests:
        cpu: '0.1'
    securityContext:
      privileged: false
    volumeMounts:
      - name: config-volume
        mountPath: /etc/kubernetes/my-scheduler
  hostNetwork: false
  hostPID: false
  volumes:
    - name: config-volume
      configMap:
        name: my-scheduler-config

# 생성해 준다
kubectl create -f my-scheduler.yaml
```

```bash
# A POD definition file is given. Use it to create a POD with the new custom scheduler.
# File is located at /root/nginx-pod.yaml

piVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx

# 스케쥴러 이름을 추가해 준다

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  schedulerName: my-scheduler
  containers:
  - image: nginx
    name: nginx

# pod 생성
kubectl create -f nginx-pod.yaml
```

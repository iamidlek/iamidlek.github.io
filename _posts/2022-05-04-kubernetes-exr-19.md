---
title: 쿠버네티스 - exr19 - MULTI CONTAINER PODS
author: YHole
date: 2022-05-04 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr19 MULTI CONTAINER PODS

```bash
# Identify the number of containers created in the red pod.

k describe pod red
Name:         red
Namespace:    default
Priority:     0
Node:         controlplane/10.22.117.9
Start Time:   Sat, 30 Apr 2022 13:28:35 +0000
Labels:       <none>
Annotations:  <none>
Status:       Pending
IP:
IPs:          <none>
Containers:
  apple:
    Container ID:
    Image:         busybox
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      4500
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-tdvd8 (ro)
  wine:
    Container ID:
    Image:         busybox
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      4500
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-tdvd8 (ro)
  scarlet:
    Container ID:
    Image:         busybox
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      4500
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-tdvd8 (ro)

# 3
```

```bash
# Identify the name of the containers running in the blue pod.

k describe pod blue

Containers:
  teal:
    Container ID:
    Image:         busybox
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      4500
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-mxbsw (ro)
  navy:
    Container ID:
    Image:         busybox
    Image ID:
    Port:          <none>
    Host Port:     <none>
    Command:
      sleep
      4500
    State:          Waiting
      Reason:       ContainerCreating
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-mxbsw (ro)
```

```bash
# Create a multi-container pod with 2 containers.

# Use the spec given below.
# If the pod goes into the crashloopbackoff then add the command sleep 1000 in the lemon container.

Name: yellow
Container 1 Name: lemon
Container 1 Image: busybox
Container 2 Name: gold
Container 2 Image: redis

apiVersion: v1
kind: Pod
metadata:
  name: yellow
spec:
  containers:
  - name: lemon
    image: busybox
    command:
      - sleep
      - "1000"

  - name: gold
    image: redis

k create -f sample.yaml
```

```bash
# We have deployed an application logging stack in the elastic-stack namespace. Inspect it.

k get all -n elastic-stack

NAME                 READY   STATUS    RESTARTS   AGE
pod/app              1/1     Running   0          24m
pod/elastic-search   1/1     Running   0          24m
pod/kibana           1/1     Running   0          24m

NAME                    TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)                         AGE
service/elasticsearch   NodePort   10.107.24.201    <none>        9200:30200/TCP,9300:30300/TCP   24m
service/kibana          NodePort   10.111.240.189   <none>        5601:30601/TCP                  24m

# Before proceeding with the next set of questions, please wait for all the pods in the elastic-stack namespace to be ready. This can take a few minutes.
```

```bash
# Once the pod is in a ready state, inspect the Kibana UI using the link above your terminal. There shouldn't be any logs for now.
# We will configure a sidecar container for the application to send logs to Elastic Search.
# It can take a couple of minutes for the Kibana UI to be ready after the Kibana pod is ready.

# You can inspect the Kibana logs by running:
kubectl -n elastic-stack logs kibana
```

```bash
# Inspect the app pod and identify the number of containers in it.

kubectl describe pod app -n elastic-stack
```

```bash
# The application outputs logs to the file /log/app.log.
# View the logs and try to identify the user having issues with Login.

# Inspect the log file inside the pod.

kubectl -n elastic-stack exec -it app -- cat /log/app.log
```

```bash
# Edit the pod to add a sidecar container to send logs to Elastic Search.
# Mount the log volume to the sidecar container.

# Only add a new container. Do not modify anything else. Use the spec provided below.

Name: app
Container Name: sidecar
Container Image: kodekloud/filebeat-configured
Volume Mount: log-volume
Mount Path: /var/log/event-simulator/
Existing Container Name: app
Existing Container Image: kodekloud/event-simulator

k get pods -n elastic-stack
k describe pod app -n elastic-stack

---
apiVersion: v1
kind: Pod
metadata:
  name: app
  namespace: elastic-stack
  labels:
    name: app
spec:
  containers:
  - name: app
    image: kodekloud/event-simulator
    volumeMounts:
    - mountPath: /log
      name: log-volume

  - name: sidecar
    image: kodekloud/filebeat-configured
    volumeMounts:
    - mountPath: /var/log/event-simulator/
      name: log-volume

  volumes:
  - name: log-volume
    hostPath:
      # directory location on host
      path: /var/log/webapp
      # this field is optional
      type: DirectoryOrCreate

k delete pod app -n elastic-stack
k create -f sample.yaml
```

```bash
# Inspect the Kibana UI. You should now see logs appearing in the Discover section.

# You might have to wait for a couple of minutes for the logs to populate. You might have to create an index pattern to list the logs. If not sure check this video: https://bit.ly/2EXYdHf
```

---
title: 쿠버네티스 - exr18 - SECRETS
author: YHole
date: 2022-05-03 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr18 SECRETS

```bash
# How many Secrets exist on the system?
# in the current(default) namespace

k get secrets

NAME                  TYPE                                  DATA   AGE
default-token-72c4p   kubernetes.io/service-account-token   3      5m22s
```

```bash
# How many secrets are defined in the default-token secret?

# Run the command kubectl describe secrets default-token-<id>
# and look at the data field.
# There are three secrets - ca.crt, namespace and token.

k describe secrets default-token-72c4p

Name:         default-token-72c4p
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: default
              kubernetes.io/service-account.uid: 6ccb6829-ecce-488b-90f8-7c68a47b1ee7

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     566 bytes
namespace:  7 bytes
token:      eyJhbG...
```

```bash
# What is the type of the default-token secret?

kubernetes.io/service-account-token
```

```bash
# Which of the following is not a secret data defined in default-token secret?

1. namespace
2. ca.crt
3. token
4. type

# 4
```

```bash
# The reason the application is failed is because we have not created the secrets yet.
# Create a new secret named db-secret with the data given below.
# You may follow any one of the methods discussed in lecture to create the secret.

Secret Name: db-secret
Secret 1: DB_Host=sql01
Secret 2: DB_User=root
Secret 3: DB_Password=password123

k get pods
NAME         READY   STATUS    RESTARTS   AGE
webapp-pod   1/1     Running   0          6m15s
mysql        1/1     Running   0          6m15s

k create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123
```

```bash
# Configure webapp-pod to load environment variables from the newly created secret.
# Delete and recreate the pod if required.

Pod name: webapp-pod
Image name: kodekloud/simple-webapp-mysql
Env From: Secret=db-secret


k edit pod webapp-pod

    envFrom:
    - secretRef:
        name: db-secret

k delete pod webapp-pod

k create -f /tmp/kubectl-edit-686418313.yaml

or

apiVersion: v1
kind: Pod
metadata:
  labels:
    name: webapp-pod
  name: webapp-pod
  namespace: default
spec:
  containers:
  - image: kodekloud/simple-webapp-mysql
    imagePullPolicy: Always
    name: webapp
    envFrom:
    - secretRef:
        name: db-secret
```

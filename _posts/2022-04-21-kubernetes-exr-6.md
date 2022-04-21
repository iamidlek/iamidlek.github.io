---
title: 쿠버네티스 - exr6 - LABELS AND SELECTORS
author: YHole
date: 2022-04-21 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr6 LABELS AND SELECTORS

```bash
# We have deployed a number of PODs. They are labelled with tier, env and bu. How many PODs exist in the dev environment (env)?
# Use selectors to filter the output

kubectl get pods --selector env=dev --no-headers | wc -l

or

k get pods -l env=dev --no-headers | wc -l
```

```bash
# How many PODs are in the finance business unit (bu)?

kubectl get pods --selector bu=finance --no-headers | wc -l
```

```bash
# How many objects are in the prod environment including PODs, ReplicaSets and any other objects?

kubectl get all --selector env=prod --no-headers | wc -l

or

kubectl get all -l env=prod
```

```bash
# Identify the POD which is part of the prod environment, the finance BU and of frontend tier?

kubectl get all --selector env=prod,bu=finance,tier=frontend
kubectl get all -l env=prod,bu=finance,tier=frontend
```

```bash
# A ReplicaSet definition file is given replicaset-definition-1.yaml.
# Try to create the replicaset.
# There is an issue with the file. Try to fix it.

kubectl apply -f replicaset-definition-1.yaml

- ReplicaSet: replicaset-1
- Replicas: 2

# The ReplicaSet "replicaset-1" is invalid: spec.template.metadata.labels: Invalid value: map[string]string{"tier":"nginx"}: `selector` does not match template `labels

apiVersion: apps/v1
kind: ReplicaSet
metadata:
   name: replicaset-1
spec:
   replicas: 2
   selector:
      matchLabels:
        tier: nginx
   template:
     metadata:
       labels:
        tier: nginx
     spec:
       containers:
       - name: nginx
         image: nginx

tier의 selector 값과
template의 metadata의 labels값을 같게 해준다

kubectl apply -f replicaset-definition-1.yaml
```

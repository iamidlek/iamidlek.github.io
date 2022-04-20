---
title: 쿠버네티스 - exr4 - Namespaces
author: YHole
date: 2022-04-19 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr4 Namespaces

```bash
# How many Namespaces exist on the system?

k get ns

NAME              STATUS   AGE
default           Active   3m38s
kube-system       Active   3m38s
kube-public       Active   3m38s
kube-node-lease   Active   3m38s
finance           Active   28s
marketing         Active   28s
dev               Active   28s
prod              Active   27s
manufacturing     Active   27s
research          Active   27s

# 10
```

```bash
# How many pods exist in the research namespace?

kubectl get pods --namespace=research

kubectl -n research get pods --no-headers | wc -l
# // 2
```

```bash
# Create a POD in the finance namespace.
# Use the spec given below.

# Name: redis
# Image Name: redis

kubectl run redis --image=redis -n finance
```

```bash
# Which namespace has the blue pod in it?

kubectl get pods --all-namespaces

or

kubectl get pods --all-namespaces | grep blue
```

```bash
# What DNS name should the Blue application use to access the database db-service in its own namespace - marketing.
# You can try it in the web application UI. Use port 6379.

kubectl get pods -n marketing

kubectl get svc --all-namespaces | grep marketing

marketing       blue-service      NodePort       10.43.218.228   <none>        8080:30082/TCP               10m
marketing       db-service        NodePort       10.43.51.240    <none>        6379:31609/TCP               10m

```

```bash
# What DNS name should the Blue application use to access the database 'db-service' in the 'dev' namespace
# You can try it in the web application UI. Use port 6379.

db-service.dev.svc.cluster.local
```

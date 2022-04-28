---
title: 쿠버네티스 - exr13 - Monitor Cluster Components
author: YHole
date: 2022-04-28 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr13 Monitor Cluster Components

```bash
# We have deployed a few PODs running workloads. Inspect them.
# Wait for the pods to be ready before proceeding to the next question.

# STATUS 확인
kubectl get pods

# Let us deploy metrics-server to monitor the PODs and Nodes. Pull the git repository for the deployment files.
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
```

```bash
# Deploy the metrics-server by creating all the components downloaded.
# Run the kubectl create -f . command from within the downloaded repository.

ls
# kubernetes-metrics-server

cd kubernetes-metrics-server/
# README.md                       auth-delegator.yaml  metrics-apiservice.yaml         metrics-server-service.yaml
# aggregated-metrics-reader.yaml  auth-reader.yaml     metrics-server-deployment.yaml  resource-reader.yaml

kubectl create -f .

# Metrics server deployed?

kubectl get pods -n kube-system
```

```bash
#It takes a few minutes for the metrics server to start gathering data.
# Run the kubectl top node command and wait for a valid output.
kubectl top node

NAME           CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
controlplane   323m         0%     1249Mi          0%
node01         49m          0%     348Mi           0%
```

```bash
# Identify the node that consumes the most CPU.

kubectl top node --sort-by='cpu' --no-headers | head -1
# 가장 많은 cpu 사용량을 가진 node만 출력
```

```bash
# Identify the node that consumes the most Memory.

kubectl top node --sort-by='memory' | head -2
```

```bash
# Identify the POD that consumes the most Memory.

kubectl top pod --sort-by='memory'
```

```bash
# Identify the POD that consumes the least CPU.

kubectl top pod --sort-by='cpu' --no-headers | tail -1
```

---
title: 쿠버네티스 - exr15 - Rolling Updates and Rollbacks
author: YHole
date: 2022-04-30 +0900
categories: [CS, Docker]
tags: [Docker]
---

# kubernetes exr15 Rolling Updates and Rollbacks

```bash
# We have deployed a simple web application. Inspect the PODs and the Services
# Wait for the application to fully deploy and view the application using the link called Webapp Portal above your terminal.

k get all
```

```bash
# Run the script named curl-test.sh to send multiple requests to test the web application. Take a note of the output.
# Execute the script at /root/curl-test.sh.

sh curl-test.sh

for i in {1..35}; do
   kubectl exec --namespace=kube-public curl -- sh -c 'test=`wget -qO- -T 2  http://webapp-service.default.svc.cluster.local:8080/info 2>&1` && echo "$test OK" || echo "Failed"';
   echo ""
done
```

```bash
# Inspect the deployment and identify the number of PODs deployed by it

k describe deployments

Name:                   frontend
Namespace:              default
CreationTimestamp:      Tue, 26 Apr 2022 05:06:20 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               name=webapp
Replicas:               4 desired | 4 updated | 4 total | 4 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        20
RollingUpdateStrategy:  25% max unavailable, 25% max surge

# desired가 4
```

```bash
# What container image is used to deploy the applications?

Pod Template:
  Labels:  name=webapp
  Containers:
   simple-webapp:
    Image:        kodekloud/webapp-color:v1 # 사용한 이미지
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
```

```bash
# Inspect the deployment and identify the current strategy

RollingUpdateStrategy:  25% max unavailable, 25% max surge
```

```bash
# If you were to upgrade the application now what would happen?

# 1. All PODs are taken down before upgrading any
# 2. PODs are upgraded few at a time

2
```

```bash
# Let us try that. Upgrade the application by setting the image on the deployment to kodekloud/webapp-color:v2
# Do not delete and re-create the deployment. Only set the new image name for the existing deployment.

kubectl edit deployment frontend
# 이미지 버전을 v2로 변경
deployment.apps/frontend edited

```

```bash
# Run the script curl-test.sh again. Notice the requests now hit both the old and newer versions. However none of them fail.
# Execute the script at /root/curl-test.sh.

sh curl-test.sh
```

```bash
# Up to how many PODs can be down for upgrade at a time
# 한번에 업그레이드 가능한 pod의 수
# Consider the current strategy settings and number of PODs - 4

# Look at the Max Unavailable value under RollingUpdateStrategy in deployment details

RollingUpdateStrategy:  25% max unavailable, 25% max surge

4분의 1이 max unavailable이므로 1개씩 교체
```

```bash
# Change the deployment strategy to Recreate

# Delete and re-create the deployment if necessary. Only update the strategy type for the existing deployment.

Deployment Name: frontend
Deployment Image: kodekloud/webapp-color:v2
Strategy: Recreate

kubectl edit deployment frontend

apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: webapp
  strategy:
    type: Recreate # 업데이트 전략 수정
  template:
    metadata:
      labels:
        name: webapp
    spec:
      containers:
      - image: kodekloud/webapp-color:v2
        name: simple-webapp
        ports:
        - containerPort: 8080
          protocol: TCP


k describe deployments.apps

Name:               frontend
Namespace:          default
CreationTimestamp:  Tue, 26 Apr 2022 05:06:20 +0000
Labels:             <none>
Annotations:        deployment.kubernetes.io/revision: 2
Selector:           name=webapp
Replicas:           4 desired | 4 updated | 4 total | 4 available | 0 unavailable
StrategyType:       Recreate
MinReadySeconds:    20
```

```bash
#

```

```bash
# Upgrade the application by setting the image on the deployment to kodekloud/webapp-color:v3

# Do not delete and re-create the deployment. Only set the new image name for the existing deployment.

kubectl edit deployments.apps frontend
# 이미지 버전 변경 kodekloud/webapp-color:v3
```

```bash
# Run the script curl-test.sh again. Notice the failures. Wait for the new application to be ready. Notice that the requests now do not hit both the versions

# Execute the script at /root/curl-test.sh

```

---
title: 쿠버네티스 - 컨테이너간 통신1
author: YHole
date: 2022-04-05 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Pod 컨테이너간 통신1

- Pod 안에 서로 다른 컨테이너끼리 localhost로 통신
  - 하나의 Pod에 서로 다른 포트로 컨테이너 2개
- 서로 다른 Pod끼리 Pod IP로 통신
  - Pod A의 컨테이너에서 Pod B의 컨테이너로 요청 전송/응답

## 흐름

1. Pod 선언과 환경 변수 설정
2. Pod 생성/배포
3. Pod IP 할당 및 컨테이너 실행 확인
4. 컨테이너 환경 변수 목록 확인
5. 컨테이너 간 localhost 통신
6. 다른 Pod의 Pod IP로 통신
7. 포트포워딩을 통해 각 컨테이너로 요청/응답 확인

## 실습 설정 개요

- 각 컨테이너의 엔드포인트

```text
/sky, /tree, /rose, /hello엔드포인트응답

const POD_IP = process.env.POD_IP
const NODE_NAME = process.env.NODE_NAME
const NAMESPACE = process.env.NAMESPACE

app.get('/sky', (req, res) => {
res.render('sky', { podIp: POD_IP, nodeName: NODE_NAME, namespace: NAMESPACE })
})

app.get('/hello', (req, res) => {
res.json("Hello, I'm Sky.")
})

app.listen(PORT, () => {
console.log(`Server is running on ${PORT}`)
})
```

- blue-app 컨테이너 설정

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: blue-green-app
spec:
  containers:
    - name: blue-app
    image: yoonjeong/blue-app:1.0
    ports:
      - containerPort: 8080
    resources:
      limits:
        memory: 64Mi
        cpu: 250m
```

- green-app 컨테이너 설정

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: blue-green-app
spec:
  containers:
    - name: green-app
    image: yoonjeong/blue-app:1.0
    ports:
      - containerPort: 8081
```

- red-app 컨테이너 설정

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: red-app
spec:
  containers:
    - name: blue-app
    image: yoonjeong/blue-app:1.0
    ports:
      - containerPort: 8080
```

### 각 환경변수 설정

```yaml
spec:
  containers:
    - env:
      - name: NODE_NAME
      valueFrom:
        fieldRef:
          fieldPath: spec.nodeName
      - name: NAMESPACE
      valueFrom:
        fieldRef:
          fieldPath: metadata.namespace
      - name: POD_IP
      valueFrom:
        fieldRef:
          fieldPath: status.podIP
```

### 명령어

```bash
# Pod 생성:
kubectl apply ‒f <yaml파일경로>

# Pod 실행및IP 확인:
kubectl get pod ‒o wide

# Pod 종료:
kubectl delete pod --all or kubectl delete pod <pod-name>

# 컨테이너간통신:
kubectl exec <pod-name> -c <container-name> -- curl -s
localhost:<container-port>

# Pod 간통신:
kubectl exec <pod-name> -c <container-name> -- curl -s <podip>:<
container-port>

# 컨테이너로그출력:
kubectl logs <pod-name> <container-name>

# 컨테이너IP 확인:
kubectl exec <pod-name> -c <container-name> -- ifconfig eth0

# 컨테이너환경변수확인:
kubectl exec <pod-name> -- printenv

# 포트포워딩:
kubectl port-forward <pod-name> <host-port>:<container-port>
```

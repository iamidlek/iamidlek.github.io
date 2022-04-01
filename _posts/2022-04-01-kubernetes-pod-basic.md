---
title: 쿠버네티스 - pod
author: YHole
date: 2022-03-31 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Pod

## 개념

- 여러 컨테이너를 감싸고 있는 콩껍질과 같다
  - 콩 = 컨테이너
- 노드에서 컨테이너를 실행하기 위한 가장 기본적인 배포 단위
- 여러 노드에서 1개 이상의 Pod을 분산 배포/실행 가능 (Pod Replicas)

## 특징

- Pod을 생성할 때 노드에서 유일한 IP를 할당 (서버 분리 효과)
- Pod 내부 컨테이너 간에 localhost로 통신 가능(포트 충돌 주의)
- Pod과 Pod은 서로의 유일한 IP로 통신
- Pod 안에서 네트워크와 볼륨 등 자원을 공유

### PodIP

- 클러스터 안에서만 접근할 수 있다
- 클러스터 외부 트래픽을 받기 위해서는 Service 혹은 Ingress 오브젝트가 필요

### Pod 복제본 생성

```bash
# orderapp 이라는 어플을 3개 복제
kubectl scale deployment orderapp --replicas=3

kubectl get pod
```

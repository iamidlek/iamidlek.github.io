---
title: 쿠버네티스 - pod
author: YHole
date: 2022-04-01 +0900
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

## Pod 안의 컨테이너 결정

1. 컨테이너들의 라이프사이클이 같은가?

- Pod 라이프사이클 = 컨테이너들의 라이 프사이클
- 컨테이너A가 종료되었을 때 컨테이너 B 실행이 의미가 있는가

2. 스케일링 요구사항이 같은가?

- 웹서버 vs 데이터베이스, 트래픽이 많은 vs 그렇지않은

3. 인프라 활용도가 더 높아지는 방향으로

- 쿠버네티스가 노드 리소스 등 여러상태를 고려하여 Pod을 스케쥴링
- Pod를 너무 크게 만들면 노드 리소스의 빈공간을 찾기 어려운 경우 발생

## 노드에 배포되는 과정

1. 사용자로부터 Pod 배포 요청을 수락한다
2. 요청받은 수만큼 Pod Replica를 생성한다(Pod desired state == current state)
3. Pod을 배포할 적절한 노드를 선택한다(nodeSelector)
4. 5에게 이미지 다운로드를 명령하고 Pod 실행을 준비한다. Pod 상태를 업데이트한다.
5. 이미지를 다운로드하고 컨테이너를 실행한다.

- Master Node
  - API Server
  - Replication Controller
  - Scheduler
- Worker Node
  - kubelet
  - Container Runtime(Docker)

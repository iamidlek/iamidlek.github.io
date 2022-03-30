---
title: 쿠버네티스 - 오브젝트
author: YHole
date: 2022-03-28 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 쿠버네티스로 컨테이너 배포 관리

## 사용자 의도

- 어떤 어플리케이션을 얼마나 어디에 어떤 방식으로 배포하는가
- 표현 방식 : YAML
- 전달 방식 : REST API
- 의도를 정의하는 방법 : 쿠버네티스 오브젝트
  - 오브젝트 종류에 따라 정의할 수 있는 속성이 달라진다.
  - 오브젝트 정의의 종류에 따라 쿠버네티스 상태가 결정된다.

## 쿠버네티스 오브젝트

> 클러스터(서버의 집합)를 이용해 애플리케이션을 배포하고 운영하기 위해 필요한 모든 쿠버네티스 리소스

### 오브젝트가 될 수 있는 것(클러스터의 상태를 표현하는 개체들)

- Pod : 어떤 어플리케이션을
- ReplicaSet : 얼마나
- Node, Namespace : 어디에
- Deployment : 어떤 방식으로 배포할 것인가
- Service, Endpoints : 트래픽을 어떻게 로드밸런싱할 것인가

```text
우리클러스터에는10(서버)개의Node에5개의Namespace가있고, 100개의Deployment를이용해 애플리케이션을배포하고있구나.
배포는점진적배포전략을이용하고있네.
ReplicaSet를보니Pod를2개씩생성해서애플리케이션을실행하고있구나.
Service를 보니 이
서비스를호출하려면my-app이라는도메인이름으로호출할수있겠다.
```

### 개발자의 의도를 담는 spec 필드

- 오브젝트기본정보(필수값)
- apiVersion: 오브젝트를생성할때사용하는API 버전
- kind: 생성하고자하는오브젝트종류
- metadata: 오브젝트를구분지을수있는정보
  - name, resourceVesion, labels, namespace, …
- spec: 사용자가원하는오브젝트상태 선언할수있는속성은오브젝트종류마다다르다
  - [kubernneties-api](https://kubernetes.io/docs/reference/kubernetes-api/)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

### status필드

> 쿠버네티스가 오브젝트 상태를 알려주는 필드

```text
오브젝트가 쿠버네티스 클러스터에 생성되면
쿠버네티스는 오브젝트정보에 status 필드를 추가하고
현재 실행중인 오브젝트의 상태 정보를 알려준다
```

- spec : 쿠버네티스가 달성해야 할 목표
- status : 오브젝트의 현재 상태

1. 사용자가 쿠버네티스 오브젝트 YAML파일을작성한다(spec 작성)
2. 쿠버네티스 API를 이용해서 쿠버네티스에 생성을 요청한다
3. 쿠버네티스 API server가 오브젝트파일의 spec 파일을 읽고 오브젝트를 생성한다
4. 쿠버네티스 ControllerManager가 spec과status를 비교하면서 계속 조정하고 상태를 업데이트한다

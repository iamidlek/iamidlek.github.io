---
title: 쿠버네티스 - label, selector
author: YHole
date: 2022-04-07 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Label과 Selector 개념

- Label(레이블)

  - 쿠버네티스 오브젝트를 식별하기위한 key/value 쌍의 메타정보

- Selector 개념

  - Label을 이용해 쿠버네티스리소스를 필터링하고 원하는 리소스 집합을 구하기 위한label query

## 언제 필요한가

> 클러스터에서 서로 다른 팀의 수백 개 Pod이 동시에 실행되고 있는 상황에서
> 주문 트래픽을 주문 Pod으로, 배달 트래픽을 배달 Pod으로 라우팅해야 할 때

> 꽃배달 기능 추가로 배달 트래픽이 증가되는 상황에서
> 클러스터에서 실행 중인 배달 관련 Pod들을 수평 확장해야 할 때

> 우리가 어떤 리소스를 선택해서 명령을 실행하고자 할 때
> Label과 Selector를 이용한다

- Label: 쿠버네티스 리소스를 논리적인 그룹으로 나누기 위해 붙이는 이름표
- Selector: Label를 이용해 쿠버네티스 리소스를 선택하는 방법(label query)

## 표현과 사용

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    # 3가지로 해당 Pod을 특정할 수 있다
    app: backend
    version: v1
    env: prod
spec:
  containers:
    - image: my-pod
      name: my-pod
```

- 명령어

```bash
# my-pod 리소스에 레이블 추가
kubectl label pod my-pod app=backend
# 레이블 확인
kubectl get pod my-pod --show-labels

# 레이블 추가
kubectl label pod my-pod version=v1
# version 값 수정하기
kubectl label pod my-pod version=v2 --overwrite

# 특정 레이블의 키값만 입력해서 보기
kubectl get pod/my-pod --label-columns app,env
kubectl get pod/my-pod -L app,env # 축약형

# 키값 뒤에 - 를 붙여 해당 키: 값 을 삭제
kubectl label pod/my-pod app-
# app 레이블만 삭제됨
kubectl get pod/my-pod --show-labels
```

## Selector

```bash
kubectl get <오브젝트타입> --selector <label query 1,…, label query N>
# 축약
kubectl get <오브젝트타입> -l <label query 1,…, label query N>
# label query: key=value

# = 또는 != 연산자로 검색 가능
kubectl get pod --selector env=prod
kubectl get pod --selector app!=backend,env= prod
```

### Set-Based Selector

- 값이어떤집합에속해있다/속해있지않다(OR연산가능)
  - ‘key in (value1, value2, …)’: key의값이value1이거나value2일때
  - ‘key notin (value1, value2, …)’: key값이value1이아니거나value2가아닐때
- 키가존재한다/존재하지않는다
  - ‘key’: label에key가있을때
  - ‘!key’: label에key가없을때

```bash
# env key 값이 dev,stage,prod 중 하나일 경우
kubectl get pod --selector ‘env in (dev,stage,prod)’
kubectl get pod --selector ‘env notin (dev,stage,prod)’

# env key 값이 있는 것
kubectl get pod --selector env
# env key 값이 없는 것
kubectl get pod --selector ‘!env’

kubectl get pod --selector ‘app=backend,env in (dev,stage)’
```

## 정리

- 쿠버네티스 오브젝트 metadata.labels 속성으로 리소스에 Label을 추가할 수 있다.
- Label은 key/value 쌍으로 선언한다.
- 쿠버네티스 오브젝트에 선언한 Label의key, value를기준으로 원하는 리소스를 필터링할 수 있다.
- 필터링하기 위한 조건을 Selector로 정의한다. (label query)
- 즉 Label과 Selector를 사용하면 특정 리소스들의 집합을 구할 수 있다.
- kubectl get 명령어를 사용할 때 label query를 지정하기 위해 --selector 또는 ‒l 옵션을 사용한다.

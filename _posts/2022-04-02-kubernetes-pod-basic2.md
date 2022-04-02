---
title: 쿠버네티스 - pod 2
author: YHole
date: 2022-04-02 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Pod

## 오브젝트 표현 방법

```yaml
apiVersion: v1 # Kubernetes API버전

kind: Pod #오브젝트타입

metadata: #오브젝트를유일하게식별하기위한정보
  name: kube-basic # 오브젝트이름
  labels: #오브젝트집합을구할때사용할이름표
  app: kube-basic
  project: fastcampus

spec: #사용자가원하는오브젝트의바람직한상태
  nodeSelector: # Pod을배포할노드
  containers: # Pod 안에서실행할컨테이너목록
  volumes: #컨테이너가사용할수있는볼륨목록
```

### nodeSelector

```yaml
spec:
  nodeSelector: # Pod 배포를위한노드를선택
    gpu: “true” #노드집합을구하기위한식별자(key: value)
```

### containers

- 컨테이너 정보

```yaml
spec:
  containers:
    - name: kube-basic #컨테이너이름
      image: kube-basic:1.0 #도커이미지주소
      imagePullPolicy: “Always” #도커이미지다운로드정책(Always/IfNotPresent/Never)
      ports:
    - containerPort: 80 # 통신에사용할컨테이너포트
      # 도커파일 등에 선언한 포트와 맞게 매핑해주어야 한다.
```

- 환경 변수

```yaml
spec:
  containers:
    - name: kube-basic
      image: kube-basic:1.0
  env: # 컨테이너에설정할환경변수목록
    - name: PROFILE # 환경변수이름
      value: production # 환경변수값
    - name: LOG_DIRECTORY
      value: /logs
    - name: MESSAGE
      value: This application is running on $(PROFILE) # 다른환경변수참조
```

- 마운트 할 볼륨

```yaml
spec:
  containers:
    - name: kube-basic
      image: kube-basic:1.0
      volumeMounts: # 컨테이너에서사용할Pod볼륨목록
        - name: html # Pod 볼륨이름
          mountPath: /var/html #마운트할컨테이너경로
    - name: web-server
      image: nginx
      volumeMounts:
        - name: html
          mountPath: /usr/share/nginx/html # 같은Pod볼륨을다른경로로마운트
          readOnly: true
```

### Pod Volume

```yaml
spec:
  containers:
  volumes: # 컨테이너가사용할수있는볼륨목록
    - name: host-volume #볼륨이름
      hostPath: #볼륨타입, 노드에있는파일이나디렉토리를마운트하고자할때
        path: /data/mysql
```

- Pod 볼륨 라이프라이크= Pod 라이프 사이클
- Container에서 볼륨 마운트 방법: volumeMounts 속성
- 목적에 맞는 볼륨 선택(hostPath, gitRepo, ConfigMap, Secret, …)
- 지원하는 볼륨 타입https://kubernetes.io/docs/concepts/storage/volumes

### Self-Healing

1. Pod이 나도 모르는 사이에 종료되었다면?

- 자가치유능력(Self-Healing)이 없음, Pod이나 노드 이상으로 종료되면 끝
- 사용자가 선언한 수만큼 Pod을 유지해주는 ReplicaSet오브젝트 도입

2. Pod IP는 외부에서 접근할 수 없다, 그리고 생성할 때마다 IP가 변경된다.

- 클러스터“외부에서 접근”할 수 있는“고정적인 단일 엔드 포인트”필요
- Pod 집합을 클러스터 외부로 노출하기 위한 Service 오브젝트 도입

## 정리

### Pod생성과배포

- Pod은 여러 개의 컨테이너를 포함할 수 있고 하나의 노드에 배포된다
- Pod을 YAML 파일로 정의해 두면 필요할 때 원하는 수만큼 노드에 배포할 수 있다
- Pod과 컨테이너를 1:1로 기본설계하고 특별한 사유가 있는 경우 1:N 구조를 고민한다

### Pod IP

- 쿠버네티스는 Pod을 생성할 때 클러스터 내부에서만 접근할 수 있는 IP를 할당한다
- Pod IP는 컨테이너와 공유되기 때문에 컨테이너 간 포트 충돌을 주의해야 한다
- 하나의 Pod에 속한 컨테이너들은 localhost로 통신할 수 있다.
- 다른 Pod(컨테이너)과 통신은 Pod IP를 이용한다

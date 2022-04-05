---
title: 쿠버네티스 - pod env
author: YHole
date: 2022-04-04 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Pod 컨테이너 환경변수

## 흐름

1. Pod 선언과환경변수설정
2. Pod 생성/배포
3. Pull image(Docker)
4. Pod IP할당및컨테이너실행확인
5. Port-forward 3000:3000
6. Pod으로트래픽을전송
7. HTTP 서버응답확인
8. 컨테이너환경변수목록확인

## 작성해 보기

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-app # pod 이름
spec:
  containers:
    - name: hello-app # 컨테이너 이름
      image: devchloe/hello-app:1.0 # 실습용 이미지
      ports:
        - containerPort: 3000 # 3000번 포트로 요청이 오면 포워딩
```

- 커스텀한 환경변수

```yaml
spec:
containers:
  - env: hello-app
    - name: STUDENT_NAME #환경변수키선언
    value: 홍길동 #환경변수값선언
    - name: GREETING
    value: 쿠버네티스입문강의에오신것을환영합니다. $(STUDENT_NAME)님!
```

- Pod오브젝트값을환경변수값으로설정

```yaml
spec:
containers:
  - env: hello-app
    - name: NODE_NAME
    valueFrom: # ‘쿠버네티스오브젝트로부터환경변수값을얻겠다’
      fieldRef: # ‘Pod spec, status의field를환경변수값으로참조하겠다’
        fieldPath: spec.nodeName #참조할field의경로선택
    - name: NODE_IP
    valueFrom:
      fieldRef:
        fieldPath: status.hostIP
```

## 명령어

```yaml
- Pod 생성: kubectl apply ‒f <yaml파일경로>
- Pod 실행및IP 확인: kubectl get pod ‒o wide
- Pod 종료: kubectl delete pod --all or kubectl delete pod <pod-name>
- 컨테이너IP 확인: kubectl exec <pod-name> [-c <container-name>] -- ifconfig eth0
- 컨테이너환경변수확인: kubectl exec <pod-name> -- env
- 포트포워딩: kubectl port-forward <pod-name> <host-port>:<container-port>
```

## 실습

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-app
spec:
  containers:
    - name: hello-app
      image: yoonjeong/hello-app:1.0
      ports:
        - containerPort: 8080
      env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name # pod의 field 경로
        - name: POD_IP
          valueFrom:
            fieldRef:
              fieldPath: status.podIP
        - name: NAMESPACE_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
        - name: NODE_IP
          valueFrom:
            fieldRef:
              fieldPath: status.hostIP
        - name: STUDENT_NAME
          value: yanagi
        - name: GREETING
          value: 쿠버네티스 입문 강의에 오신 것을 환영합니다. $(STUDENT_NAME)님!
      resources:
        limits:
          memory: "128Mi"
          cpu: "100m"
```

```sh
# Pod 생성
kubectl apply -f hello-app.yaml

# Pod 실행 및 IP 확인
kubectl get pod -o wide
kubectl get pod/hello-app -o json

# 컨테이너 IP 확인
kubectl exec hello-app -- ifconfig eth0
kubectl exec hello-app -- cat /etc/hosts

# 컨테이너에 설정된 환경변수 확인 (env)
kubectl exec hello-app -- env

# 컨테이너가 리스닝하고 있는 포트 확인
kubectl exec hello-app -- netstat -an

# 로컬 포트 포워딩  (8080 -> 8080)
kubectl port-forward hello-app 8080:8080

# HTTP Server 응답 확인  (웹브라우저 or curl)
curl -v localhost:8080

# Pod 종료
kubectl delete pod --all
# kubectl delete pod hello-pod
```

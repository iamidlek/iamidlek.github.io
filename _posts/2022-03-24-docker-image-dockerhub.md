---
title: 도커 이미지 - 도커 허브
author: YHole
date: 2022-03-24 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 허브

- [도커 허브](https://hub.docker.com/)

## account setting

- security 에서 access token을 권한에 맞게 발행

```bash
docker login -u 아이디
# password => 토큰 붙여넣기

# 로그인 성공 후
cat /home/ubuntu/.docker/config.json
# 토큰 값이 적혀 있기 때문에 관리를 잘 해주어야 한다.
```

## create repository

- 레포지토리 생성 하기

- 이미지 올리기

```bash
# 이미지를 태깅
docker tag nginx:latest 도커아이디/레포지토리:v1.0.0
# 올리기
docker push 도커아이디/레포지토리:v1.0.0

# 확인하기
docker rmi 도커아이디/레포지토리:v1.0.0
docker pull 도커아이디/레포지토리:v1.0.0
```

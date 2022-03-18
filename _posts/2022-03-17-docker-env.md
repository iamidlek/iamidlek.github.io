---
title: 도커 컨테이너 다루기(env, exec)
author: YHole
date: 2022-03-17 +0900
categories: [CS, Docker]
tags: [Docker]
---

## env 환경변수

- MY_HOST 라는 변수 설정하기

```bash
docker run -i -t -e MY_HOST=sample.com ubuntu:focal bash
```

### 파일로 주입하기(개발 환경)

- 환경변수를 파일로 가져오기 확인 방법

```bash
docker run -i -t --env-file ./sample.env ubuntu:focal env
```

> 도커 이미지에서 nginx 등 공식 문서에서 설정을 위한  
> env 변수 컨트롤 설명을 참조해서 사용하면 좋다

## 실행중인 컨테이너에 명령어 실행하기

```bash
docker exec 컨테이너명 명령어
```

```bash
# my-nginx로 백그라운드에 띄우기
docer run -d --name my-nginx nginx
# 환경변수 확인
docker exec my-nginx env
# bash 셀에 접속
docker exec -i -t my-nginx bash
```

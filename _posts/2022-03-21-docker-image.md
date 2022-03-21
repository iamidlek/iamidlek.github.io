---
title: 도커 이미지 - 기초
author: YHole
date: 2022-03-21 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 이미지

## 구조

- layer 구조

```text
# 도커 이미지 예시 (하단으로 갈 수록 오래된 변경)
web app source
nginx
layer C   ┐
layer B   ├ ubuntu
layer A   ┘

# 컨테이너 예시
R / W layer
(readonly)image layer
```

## 이미지 정보 확인하기

```bash
docekr images

docker image inspect nginx:latest
```

## 도커파일 없이 이미지 생성

```bash
docker run -it --name sample ubuntu:focal
# 샘플 파일
cat > sample_file
# 내용 입력후 ctrl p q (실행 상태에서 빠져 나오기)

# 변경사항 저장하기
# -a: 누가 변경했는지
# -m: 커밋 메세지
# 컨테이너명 새로 만들 이미지명
docker commit -a yoo -m "Add sample_file" sample sample:v1

docker images
docker image inspect sample:v1

# 기존 컨테이너 종료 후 새로만든 이미지 실행
docker rm -f sample
docker run -i -t sample:v1
# ls 해보면 만든 sample_file이 존재
```

## Dockerfile을 이용하여 이미지 생성

```bash
# docker build [options] [path]

# ./ 디렉토리를 빌드 컨텍스트로 이미지 빌드(Dockerfile 이용)
docker build -t sample:v1 ./

# ./ 디렉토리를 빌드 컨텍스트로 이미지 빌드(example/MyDockerfile 이용)
docker build -t sample:v1 -f example/MyDockerfile ./
```

- Docker daemon  
  using cache => 레이어 케싱 : 동일한 동작은 다시 하지 않음

- 빌드 컨텍스트(Build Context)란?

> 도커 빌드 명령 수행 시의 Current Working Directory를 의미
> Dockerfile로 부터 이미지 빌드에 필요한 정보를 도커 데몬에게 전달하기 위한 목적으로 존재

- .dockerignore

> 특정 디렉토리 혹은 파일 목록을 빌드 컨텍스트에서 제외한다  
> 용량이 큰 것들이 빌드 컨텍스트에 포함되면 빌드 시간이 늘어난다

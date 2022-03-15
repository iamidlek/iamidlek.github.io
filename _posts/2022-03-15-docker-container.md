---
title: 도커 컨테이너
author: YHole
date: 2022-03-15 +0900
categories: [CS, Docker]
tags: [Docker, container]
---

# Docker-container

## 라이프 사이클

- created => (start) => running
- (run) => running
- running => (pause/unpause) => paused
- running => (stop/start) => stopped
- created/stopped => (rm) => deleted

## 컨테이너 시작

```bash
docker ps # 실행중인 컨테이너 목록
docker ps -a # 전체 컨테이너 목록
docker inspect [container id or name] # 컨테이너 상세 정보

docker create nginx # 생성

# 생성및 시작
docker run nginx # 이미지 없으면 자동으로 pull해서 실행
```

- 컨테이너 시작하는 두가지 방법

```bash
# 컨테이너 목록의 NAMES로 실행하는 방법
docker start [NAMES]
# 컨테이너의 아이디로 실행하는방법
docker start [container id]
```

## 주요 옵션

```bash
-i # 호스트의 표준 입력을 컨테이너와 연결(interactive)
-t # 터미널의 명령들을 컨테이너에 전달 TTY할당

--rm # 컨테이너가 실행 종료되면 자동 삭제됨
-d # 백그라운드 모드로 실행 (detached)

--name hello-hello # 컨테이너 이름 지정 가능
# docker run -d --name new-nginx nginx

-p 80:80 # 호스트 - 컨테이너 간 포트 바인딩
# docker run -p 80:80 -d nginx
# curl localhost:80
# 확인 가능

-v /opt/example:/example # 호스트 - 컨테이너 간 볼륨 바인딩

fc/hello:latest # 실행할 이미지 정보
my-command # 컨테이너 내에서 실행할 명령어
```

### 멈추기

```bash
# 일시 중지
docker pause [container id or name]
docker unpause [container id or name]
```

```bash
# 종료 sigterm
docker stop [container id or name]
# 강제 종료 sigkill
docker kill [container id or name]

# 전체 컨테이너 종료
docker stop $(docker ps -a -q)
```

### 삭제하기

```bash
# 실행중인 컨테이너는 못지움
docker rm [container id or name]

# 강제 종료 후 삭제
docker rm -f [container id or name]

# 컨테이너 실행이 종료되면 자동 삭제
docker run --rm

# 중지된 모든 컨테이너 삭제
docker container prune
```

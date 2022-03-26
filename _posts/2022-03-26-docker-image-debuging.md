---
title: 도커 - 데몬 디버깅
author: YHole
date: 2022-03-26 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 데몬 디버깅

```bash
docker ps -a
docker system
docker system info

# 시스템 이벤트
docker system events -h
# 동일한 명령
docker events -h

# 실행하면 스트리밍 형태로 이벤트들이 터미널에 보여짐
docker events

# 추가 터미널에서 실행해서 확인
docker run --name my-nginx nginx:latest
```

- 사용량 확인

```bash
# 이미지, 컨테이너 사용량 확인
docker system df
# 디테일한 확인
docker system df -v

# 중지된 컨테이너 등 제거 가능
docker system prune

# 각 컨테이너별 cpu 네트워크i/o 등 확인가능
docker stats
```

## 우분투

```bash
# OS에 따라 확인 방법이 다르다
journalctl -u docker
```

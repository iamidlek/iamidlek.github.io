---
title: 도커 이미지 - 경량화
author: YHole
date: 2022-03-25 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 이미지 경량화

- 필요한 패키지 및 파일만 추가
- 컨테이너 레이어 수 줄이기

```dockerfile
FROM alpine:3.14
LABEL maintainer="sample name<samplename@naver.com>"
LABEL description="sample desc"

# &&로 레이어를 줄이고
# --nocache로 크기를 줄인다
RUN \
  apk add --nocache bash curl git && \
  git clone https://github.com/course-hero/slacktee /slacktee && \
  apk del --no-cache git
RUN chmod 755 /slacktee/slacktee.sh

WORKDIR /slacktee
ENTRYPOINT ["/bin/bash", "-c", "./slacktee.sh"]
```

## 경량 베이스 이미지 선택

- node:lastest => 905MB
- node:16-slim => 174MB
- node:16-alpine => 110MB

## 멀티 스테이지 빌드 사용

- 멀티 스테이지 파이프라인

```dockerfile
# AS를 사용하여 하나의 블록으로 구현
FROM node:16-alpine AS base
LABEL maintainer="sample name<samplename@naver.com>"
LABEL description="sample desc"

WORKDIR /app

COPY package*.json ./

# 빌드 블록
FROM base AS build
RUN npm install

# 릴리즈 블록
FROM base AS release
COPY --form=build /app/node_modules ./node_modules
COPY . .

EXPOSE 8080
CMD [ "node", "server.js"]

# base에 변경이 없으면 base는 변경 없음
```

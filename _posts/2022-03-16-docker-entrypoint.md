---
title: 도커 엔트리포인트와 커멘드
author: YHole
date: 2022-03-16 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Entrypoint & Command

```dockerfile
FROM node:alpine
WORKDIR /usr/app

RUN npm install --global pm2

COPY package.json package-lock.json ./

RUN npm install

COPY ./ ./

RUN npm run build

EXPOSE 3000

USER node

# ENTRYPOINT [] 선택
# CMD 필수
CMD [ "pm2-runtime", "npm", "--", "start" ]
```

## Entrypoint

- 도커 컨테이너가 실행될 때 고정적으로 실행되는 스크립트(또는 명령어)
  생략하면 커맨드에 지정된 명력어로 수행

## Command

- 도커 컨테이너가 실행될 때 수행할 명령어(엔트리 포인트에 지정된 명령어에 대한 인자 값)

```bash
# 이미지의 내용을 오버라이드
# bash 커멘드가 기본이지만 sh로 변경
docker run --entrypoint sh -i -t ubuntu:focal
# echo 명령어가 실행되도록 하는 것도 가능
docker run --entrypoint echo ubuntu:focal hello world
```

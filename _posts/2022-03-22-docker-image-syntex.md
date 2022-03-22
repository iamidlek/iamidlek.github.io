---
title: 도커 이미지 - 문법
author: YHole
date: 2022-03-22 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 문법

## 의미들

```dockerfile
FROM node:16
LABEL maintainer="name <email>"
LABEL description="desc"
# cd처럼 working directory를 지정
WORKDIR /app
# SRC(소스(호스트) 경로) DEST 목적지 이미지 상에서의 경로
COPY package*.json ./
# 결론적으로 /app에 복사
RUN npm install
# 현재 디렉토리에서의 모든 소스를 ./app으로 복사
COPY . .
# 8080포트를 사용한다고 문서화
# -p로 포트바인딩 하지 않으면 의미는 없다
EXPOSE 8080
CMD [ "node", "server.js"]
```

### 변수 사용

```dockerfile
# ${} => 컨테이너의 환경변수를 사용하는 방법
ENV TEST=/bar
WORKDIR ${TEST}
# WORKDIR /bar
```

- ARG를 이용하는 방법

```bash
docker build --build-arg TEST=v14.8.2
```

```dockerfile
# ARG
ARG user1=someuser
ARG buildno=1
# ARG 정의 전에 사용하면
# build에서 값을 넣어도 받지 못하고 default값 사용
USER ${TEST:-some_val}
ARG TEST
```

```dockerfile
# ARG와 ENV가 동일한 변수명을 가지면
# 항상 ENV의 값으로 덮어 쓰인다
ARG TEST
ENV TEST=win
```

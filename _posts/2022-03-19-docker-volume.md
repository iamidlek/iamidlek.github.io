---
title: 도커 컨테이너 다루기 - 볼륨
author: YHole
date: 2022-03-19 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 레이어 아키텍쳐

- 이미지레이어와 컨테이너 레이더를 가지게 된다
- 컨테이너 레이어는 종료시 삭제되는 임시저장소

- docker run app(read write)(컨테이너 레이어)

  - layer 6: container layer

- docker build -t app(read only)(이미지 레이어)

  - layer 5: update entrypoint
  - layer 4: source code
  - layer 3: install in pip packages
  - layer 2: changes in apt packages
  - layer 1: base ubuntu layer

## 호스트 볼륨

- 호스트의 디렉토리를 컨테이너의 특정 결로에 마운트

```bash
# 호스트의 /opt/html 디렉토리를 nginx의 웹 루트 디렉토리로 마운트
docker run -d \
--name nginx \
# -v 호스트 - 컨테이너 간 볼륨 바인딩
-v /opt/html:/usr/share/nginx/html \
nginx
```

- 볼륨을 사용하여 데이터 변경을 남기기
- pwd 현재 워킹 디렉토리 경로를 의미

```bash
docker run \
-d \
-v $(pwd)/html:/usr/share/nginx/html \
-p 80:80\
nginx

docker exec -it 컨테이너명 bash

# 새로운 파일 만들기
cat > /usr/share/nginx/html/hello
# 호스트에도 동일한 파일이 만들어짐
```

## 볼륨 컨테이너

- 특정 컨테이너의 볼륨 마운트를 공유 할 수 있다.

```bash
# 예시.sh
docker run \
-d \
-it \
-v $(pwd)/html:/usr/share/nginx/html \ # 디렉토리 마운트
--name my-volume \
ubuntu:focal

# my-volume컨테이너의 볼륨을 공유
docker run \
-d \
--name nginx \
--volumes-from my-volume \ # --volumes-from 컨테이너 지정
-p 80:80 \
nginx

docker run \
-d \
--name nginx2 \
--volumes-from my-volume \
-p 8080:80 \
nginx
```

## 도커 볼륨

```bash
# mysql 예시
# 볼륨 생성
docker volume create --name db

docker run \
-d \
--name my-sql \
-e MYSQL_DATABASE=이름 \
-e MYSQL_ROOT_PASSWORD=비번 \
-v db:/var/lib/mysql \ # 생성해둔 볼륨과 연결
-p 3306:3306 \
mysql:5.7

# 제거 시
docker rm -f $(docker ps -a -q)
docker volume rm db
```

## 읽기 전용 볼륨 연결

- ro를 추가해준다

```bash
docker run \
-d \
-v $(pwd)/html:/usr/share/nginx/html:ro \
-p 80:80\
--name ro-nginx \
nginx

# read only이므로 에러가 나오면 성공
docker exec ro-nginx touch /usr/share/nginx/html/test
```

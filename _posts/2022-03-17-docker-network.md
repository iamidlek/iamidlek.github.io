---
title: 도커 컨테이너 다루기(expose vs publish)
author: YHole
date: 2022-03-18 +0900
categories: [CS, Docker]
tags: [Docker]
---

## 도커 네트워크 구조

- veth: virtual eth
- docker0: 도커엔진에 의해 기본 생성되는 브릿지 네트워크  
  => veth와 eth(컨테이너) 간 다리 역할
- eth0: host running docker와 각 컨테이너의 IP주소

```bash
docker run -p [host ip port]:[container port] [컨테이너명]

# 80:80 => 호스트의 모든ip의 80포트 에 컨테이너의 80번 포트를 연결
docker run -d -p 80:80 nginx

# 호스트의 127.0.0.1의 80포트 에 컨테이너의 80번 포트를 연결
docker run -d -p 127.0.0.1:80:80 nginx

# 호스트의 사용가능한 포트와 컨테이너의 80번 포트를 연결
docker run -d -p 80 nginx
```

### expose

```bash
# publish 옵션은 실제 포트를 바인딩
# expose 옵션은 문서화 용도
docker run -d --expose 80 nginx
# 요청을 보내도 커넥션이 없음
curl localhost:80
```

## 도커 네트워크 드라이버

```bash
docker network
# Container Network Model
# 1. native drivers
#    (single host networking)
#    - bridge
#    - host
#    - none
#    (multi host networking)
#    - overlay
# 2. remote drivers
#    - 3rd-party plugins
```

### 확인

```bash
# 강제 삭제
docker rm -f $(docker ps -a -q);
# 사용하지 않는 컨테이너 삭제
docker container prune
```

- none network

```bash
# 커스텀 네트워크 혹은 네트워크가 필요 없는 경우 사용

# none.sh
docker run -i -t --net none ubuntu:focal

# 스크립트 실행
./none.sh
```

- host network (호스트 네트워크라 ip주소 지정 안됨)

```bash
# host.sh
docker run -d --network-host grafana/grafana

./host.sh
curl localhost:3000
# or
docker inspect 컨테이너
# 확인가능
```

- bridge network(사용자 정의 브릿지 네트워크)

```bash
# bridge.sh
docker network create --driver=bridge sample

docker run -d --network=sample --net-alias=hello nginx
docker run -d --network=sample --net-alias=grafana grafana/grafana

./bridge.sh
docker network ls # sample 브릿지 네트워크 확인
docker ps # 두개의 컨테이너가 생긴 것을 확인

# grafana에 bash 접근
docker exec -it 컨테이너아이디 bash
cd /tmp
# hello(nginx)에서 index.html을 받아서 열어보기
wget hello
cat index.html
# nginx와 grafana가 동일한 브릿지 네트워크로 만들어 짐

# nginx에 bash 접근
docker exec -it 컨테이너아이디 bash

curl grafana:3000
```

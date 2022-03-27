---
title: 도커 - 컴포즈
author: YHole
date: 2022-03-27 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 컴포즈

> 단일 서버에서 여러 컨테이너를 프로젝트 단위로 묶어서 관리  
> docker-compose.yml YAML 파일을 통해 명시적 관리

- 프로젝트 단위로 도커 네트워크와 볼륨 관리
- 프로젝트 내 서비스 간 의존성 정의 가능
- 프로젝트 내 서비스 디스커버리 자동화
- 손 쉬운 컨테이너 수평 확장

## 사용 목적

### 로컬 개발 환경 구성

- 특정 프로젝트의 로컬 개발 환경 구성 목적으로 사용
- 프로젝트의 의존성(redis, mysql, kafka 등)을 쉽게 띄울 수 있음

### 자동화된 테스트 환경 구성

- CI/CD 파이프라인 중 쉽게 격리된 테스트 환경을 구성하여 테스트 수행 가능

### 단일 호스트 내 컨테이너를 선언적 관리

- 단일 서버에서 컨테이너를 관리할 때 YAML 파일을 통해 선언적으로 관리 가능

## 용어

### 프로젝트

- 도커 컴포즈에서 다루는 워크스페이스 단위
- 함께 관리하는 서비스 컨테이너의 묶음
- 프로젝트 단위로 기본 도커 네트워크가 생성 됨

### 서비스

- 도커 컴포즈에서 컨테이너를 관리하기 위한 단위
- scale을 통해 서비스 컨테이너의 수 확장 가능
- 서비스를 통해 컨테이너 관리

## 최상위 옵션

- version, services, networks, volumes

### version

- 가능한 최신 버전 사용을 권장
- 도커 엔진 및 도커 컴포즈 버전에 따른 호환성 매트릭스 참조가 필요
  - compose file reference 참조
- 버전 3부터 docker swarm과 호환
  - Docker Swarm : 여러 서비스를 기반으로 스왐클러스터를 형성하여  
    컨테이너를 관리하는 컨테이너 오케스트레이션 시스템  
    쿠버네티스와 동일 목적으로 만들어졌지만 인기를 끌지 못함

### services

- 프로젝트 내에 구성되는 서비스들을 키: 값 형태로 작성한다

### networks

- 정의하지 않아도 기본 네트워크가 브릿지 모드로 생성됨

### volumes

- 볼륨키에서 정의해서 사용하면 된다

## 명령어

- web은 현재 디렉토리의 dockerfile을 찾아서 빌드

```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000"
  redis:
    image: "redis:alpine"
```

- 실행

```bash
# docker run과 같은 명령
docker-compose up

# 프로젝트 명 변경 & 디테치드
docker-compose -p my-project up -d
docker-compose ls
# 중단된 목록도 포함하여 확인
docker-compose ls -a
```

- 종료

```bash
# 프로젝트 내 컨테이너 및 네트워크 종료 및 제거
docker-compose down
# 추가로 볼륨까지 지우는 경우
docker-compose down -v
```

### sample

```yaml
version: "3.9"

services:
  db:
    image: mysql:5.7
    volumes:
      - db:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=wordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    networks:
      - wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    networks:
      - wordpress

volumes:
  db: {}

networks:
  wordpress: {}
```

```bash
# 생성
docker-compose up -d
# 제거
docker-compose down
# 볼륨은 남아있던 상태
docker-compose up -d
docker-compose down -v
```

### 서비스 확장

```bash
docker-compose ps
# 해당 이름의 컨테이너 목록 확인
docker-compose -p my-project ps

# 스케일링
docker-compose -p my-project up --scale web=3
# 생선된 것 확인
docker-compose -p my-project ps
```

> 유의할 점

```bash
# 호스트 포트를 사용하면 스케일링 불가
ports:
  - "5000:5000" # 문제 발생

=>
ports:
  - "5000"

# contianer_name 지정도 하면 안된다.

web:
  contianer_name: "이름"
```

### 기타 명령어

```bash
# 프로젝트 내 서비스 로그 확인
docker-compose logs
# 프로젝트 내 컨테이너 이벤트 확인
docker-compose events
# 프로젝트 내 이미지 목록
docker-compose images
# 프로젝트 내 컨테이너 목록
docker-compose ps
# 프로젝트 내 실행중인 프로세스 목록
docker-compose top
```

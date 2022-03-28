---
title: 도커 - 컴포즈 실습
author: YHole
date: 2022-03-28 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Grafana + MySQL 구성하기

## 1단계 Grafana 구성 요구사항

- Grafana의 3000번 포트는 호스트의 3000번 포트와 바인딩
- Grafana의 설정파일인 grafana.ini는 호스트에서 주입 가능하도록 구성하고 읽기전용 설정
- Grafana의 로컬 데이터 저장 경로를 확인하여 도커 볼륨 마운트
- Grafana의 플러그인 추가 설치를 위한 환경변수 설정
- 로그 드라이버 옵션을 통해 로그 로테이팅

[Grafana 도커 가이드](https://grafana.com/docs/grafana/latest/installation/docker/)

### docker-compose.yml

```yaml
version: "3.9"

services:
  grafana:
    image: grafana/grafana:8.2.2
    # always 서버가 띄어져 있는 동안 오류가 있어 컨테이너가 실패하는 경우 재시작
    # unless-stopped 조금 더 강력한 옵션 서버가 재시작 되더라고 컨테이너를 재시작
    restart: unless-stopped
    environment:
      GF_INSTALL_PLUGINS: grafana-clock-panel
    ports:
      - 3000:3000
    volumes:
      # /etc/grafana/grafana.ini 기본적으로 grafana가 불러오는 경로
      # ./files/grafana.ini 경로에 마운트 ro => 읽기 전용
      - ./files/grafana.ini:/etc/grafana/grafana.ini:ro
      # 밑에 선언한 grafana-data 볼륨을 로컬의/var/lib/grafana에 마운트
      - grafana-data:/var/lib/grafana
    logging:
      driver: "json-file"
      options:
        max-size: "8m"
        max-file: "10"

volumes:
  grafana-data: {}
```

## 2단계 + MySQL 구성하기

- 1단계 요구사항 포함
- grafana.ini를 통해 database 설정을 sqlite에서 MySQL로 변경
- MySQL 컨테이너를 docker-compose에 db 서비스로 추가
- grafana 서비스가 db 서비스를 database로 연결하도록 구성
- MySQL의 로컬 데이터 저장 경로 확인하여 도커 볼륨 마운트

[MySQL 도커 가이드](https://hub.docker.com/_/mysql/)

```yaml
version: "3.9"

services:
  db:
    image: mysql:5.7
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: grafana
      MYSQL_DATABASE: grafana
      MYSQL_USER: grafana
      MYSQL_PASSWORD: grafana
    volumes:
      - mysql-data:/var/lib/mysql
    logging:
      driver: "json-file"
      options:
        max-size: "8m"
        max-file: "10"

  grafana:
    depends_on:
      - db
    image: grafana/grafana:8.2.2
    restart: unless-stopped
    environment:
      GF_INSTALL_PLUGINS: grafana-clock-panel
    ports:
      - 3000:3000
    volumes:
      - ./files/grafana.ini:/etc/grafana/grafana.ini:ro
      - grafana-data:/var/lib/grafana
    logging:
      driver: "json-file"
      options:
        max-size: "8m"
        max-file: "10"

volumes:
  mysql-data: {}
  grafana-data: {}
```

- grafana.ini 내용에 database부분

```ini
[database]
type = mysql
host = db:3306
name = grafana
user = grafana
password = grafana
```

- 실행

```bash
docker-compose up -d
```

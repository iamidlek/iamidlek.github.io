---
title: 도커 컨테이너 다루기 - 로그
author: YHole
date: 2022-03-20 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 로그

## STDOUT/STDERR

- app container에서 stdout/stderr로 내보내기
- logging driver중 선택하여 처리하기

## 로그 확인하기

```bash
# 로그 전체
docker logs 컨테이너

# 마지막 로그 10개
docker logs --tail 10 컨테이너

# 실시간 로그 스트림 확인
docker logs -f 컨테이너

# 로그마다 타임스탬프 표시
docker logs -f -t 컨테이너
```

## 호스트 운영체제의 로그 저장 경로

- log driver를 json-file로 하였을 경우

```bash
# 열어보는 방법
cat /var/lib/docker/containers/$(CONTAINER_ID)/$(CONTAINER_ID)-json.log
```

## 로그 용량 제한하기

- 컨테이너 단위로 로그 용량 제한도 가능
  도커엔진에서 기본적으로 적용될 설정을 하는 것도 가능하다

```bash
# 예시 : 3Mb 제한, 최대 로그 파일 3개로 로테이팅
docker run \
-d \
--log-dirver=json-file \
--log-opt max-size=3m \
--log-opt max-size=5 \
nginx
```

## 로그 드라이버

```text
json-file
journald
syslog
fluentd
ETW
splunk
gelf
logentries
amazon cloudwatch
google cloud
```

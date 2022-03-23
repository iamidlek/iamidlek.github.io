---
title: 도커 이미지 - 압축 파일
author: YHole
date: 2022-03-23 +0900
categories: [CS, Docker]
tags: [Docker]
---

# 도커 이미지 압축

## 압축 파일 만들기

- 아웃풋 경로 지정 옵션

```bash
# docker save -o [output-file] IMAGE
# ubuntu:focal 이미지를 ubuntu_focal.tar로 저장
docker save -o ubuntu_focal.tar ubuntu:focal
```

## 압축 파일 불러오기

```bash
# docker load -i [input-file]
#  ubuntu_focal.tar 압축 파일에서 ubuntu:focal 이미지 불러오기
docker load -i ubuntu_focal.tar
```

## 이미지 삭제

```bash
docker rmi 이미지
```

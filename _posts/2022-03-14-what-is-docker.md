---
title: 도커란?
author: YHole
date: 2022-03-14 +0900
categories: [CS, Docker]
tags: [Docker]
---

# Docker

- 이전의 가상 머신(VM)기술보다 효율적인 컨테이너로 격리된 환경을 구성하는 기술

| vm                  | docker           |
| ------------------- | ---------------- |
| App                 | App              |
| Bins/Libs           | Bins/Libs        |
| Guest OS            | -                |
| Hypervisor (VMware) | Docker Daemon    |
| Operating System    | Operating System |
| Physical Server     | Physical Server  |

## 도커 이미지와 컨테이너

- 가장 기본적인 단위이며 이미지로 여러 개의 컨테이너 생성이 가능하다
- 이미지와 컨테이너의 관계
  => 프로그램과 프로세스
  => 클래스와 인스턴스

### Image

- 컨테이너를 생성할 때 필요한 요소로  
  컨테이너의 목적에 맞는 바이너리와 의존성이 설치되어 있다  
  여러 레이어로 나뉘어진 바이너리 파일로 존재한다

### Container

- 호스트, 다른 컨테이너와 격리된 시스템 자원과 네트워크를 사용
  이미지는 읽기 전용으로 사용되어 변경사항은 컨테이너 계층에 저장
  컨테이너에서 변경이 일어나도 이미지는 영향을 받지 않는다.

### 도커 이미지 이름 구성

- 저장소이름/이미지이름:이미지태그
  repository/nginx:1.21
  nginx (도커 허브에서 최신버전을 가져오라는 의미)

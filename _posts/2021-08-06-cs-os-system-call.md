---
title: cs-system call
author: YHole
date: 2021-08-06 +0900
categories: [CS, Operating System]
tags: [cs, Operating System]
---

## 운영체제 구조

사용자 ⇔ 응용 프로그램 ⇔ OS ⇔ 하드웨어

> 도서관 비유
> 운영체제 = 도서관
> 응용 프로그램 = 시민
> 하드웨어 = 책

  - 시민은 도서관에 원하는 책(자원)을 요청함
  - 도서관은 적절한 책(자원)을 찾아서, 시민에게 빌려줌
  - 시민이 기한이 다 되면, 도서관이 해당 책(자원)을 회수함

- 운영체제의 역할
  - 메모리를 허가하고, 분배한다.
  - CPU 시간을 제공한다.
  - IO Devices 사용을 허가/제어

## 운영체제 제공 인터페이스

- Shell
  - 사용자가 운영체제 기능과 서비스를 조작할 수 있다
  - 쉘은 터미널 환경(CLI)과, GUI 환경 두 종류로 분류

- 응용 프로그램을 위한 인터페이스
  - API (Application Programming Interface)
    - 함수로 제공 open()
  - 보통은 라이브러리(library) 형태로 제공
    - C library

## 시스템 콜

- 운영체제에서 제공
- 운영체제 기능을 호출하는 함수
  - open()이 호출되면 sys_open()함수를 호출
- POSIX API, 윈도우 API
  - API `각 프로그래밍 언어별` 운영체제 기능 호출  
  인터페이스 함수 (각 언어별 인터페이스)

## CPU Protection Rings

- 사용자 모드 (user mode)
  - level 3 applications 응용프로그램이 사용
- 커널 모드 (kernel mode by OS)  
특권 명령어 실행과 원하는 작업 수행을 위한  
자원 접근을 가능케 하는 모드
  - level 0 kernel
  - level 1,2 OS service

- 사용자 영역
  - 응용 프로그램 ⇔ API  ⇔  `시스템 호출 인터페이스`
- 커널 영역
  - `시스템 호출 인터페이스` ⇔ 메모리, 디스크, 네트워크

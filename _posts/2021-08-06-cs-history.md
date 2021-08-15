---
title: cs-History
author: YHole
date: 2021-08-06 +0900
categories: [CS, Operating System]
tags: [cs, Operating System]
---

## 대표적인 운영체제 Operating System

- Windows, Mac
- UNIX
  - UNIX 계열 OS
    - LINUX (리눅스)

## 운영체제의 역할

1. System Resource 관리
2. 사용자와 컴퓨터간의 커뮤니케이션 지원
3. 컴퓨터 하드웨어와 응용 프로그램 제어
  - 프로그램 간의 권한 관리
  - 사용자 관리

## 간략히 보는 운영체제 역사

- 1950년대
  - 운영체제 없음
  - 응용 프로그램이 직접 시스템 자원 제어

- 1960년대
  - 운영체제 출현
  - 베치 처리 시스템

- 1960년대 후반
  - 시분할, 멀티 태스킹, 멀티 프로그래밍
  - CPU의 시간을 잘개 쪼개기
  - => 다중 사용자 지원, 응용 프로그램 동시 실행

- 1970년대
  - UNIX
  - 미국 AT&T 사의 벨 연구소
  - 데니스 리치: C언어를 개발
  - 켄톰슨, 데니스 리치
  > 70년대 이전 Assembly 언어로 소프트웨어 개발
  > 컴퓨터 마다 각각 다시 개발이 필요(불편)
  > C언어는 컴파일러가 있어 문제가 해결됨

- 1980년대
  - 대형 컴퓨터에 여럿이 접속하는 방식에서 Personal Computer로
  - CLI (Command Line Interface) 터미널에서
  - GUI (Graphical User Interface) GUI로 점점 발전 

- 1990년대
  - 다양한 응용 프로그램 시대(엑셀, 워드)
  - 네트워크 기술 발전(World Wide Web 인터넷 대중화)
  - UNIX 계열 OS + 응용 프로그램 자체 개발, 소스 오픈
  - LINUX (리눅스) 운영체제, 소스 오픈, 무료

- 2000년대 ~
  - LINUX (리눅스) 운영체제
  - Apache (아파치, 웹서버)
  - MySQL (데이터베이스)
  - 안드로이드, 딥러닝, 데이터사이언스, IoT 관련  
  가상 머신, 대용량 병렬 처리 등 활성화


## 용어 zip

#### 베치 처리 시스템 (Batch processing)
  - response time이 오래 걸릴 수 있다
  - 불필요한 시간까지 CPU를 점유 하고있을수있다

#### 시분할 시스템 (Time Sharing System)
  - 다중 사용자 지원
  - 컴퓨터 응답 시간을 최소화하는 시스템

#### 멀티 태스킹 (Multi Tasking)
  - 단일 CPU에서 여러 응용 프로그램의 병렬 실행

> 보통은 시분할 시스템 = 멀티 태스킹

#### 멀티 프로그래밍
  - 최대한 CPU를 많이 활용하도록 하는 시스템 
  - 시간대비 CPU 활용도를 높이는 방식

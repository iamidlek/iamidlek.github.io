---
title: cs-process scheduling
author: YHole
date: 2021-08-06 +0900
categories: [CS, Operating System]
tags: [cs, Operating System]
---

## 용어 복습

#### 베치 처리 시스템 (Batch processing)
  - 한번에 등록된 여러 프로그램을 순차적으로 실행 가능
  - response time이 오래 걸릴 수 있다
  - 불필요한 시간까지 CPU를 점유 하고있을수있다

#### 시분할 시스템 (Time Sharing System)
  - 다중 사용자 지원
  - 컴퓨터 응답 시간을 최소화하는 시스템

#### 멀티 태스킹 (Multi Tasking)
  - 단일 CPU에서 여러 응용 프로그램의 병렬 실행
  - 자주 실행 응용 프로그램을 변경하여 동시에 실행되는 것처럼 보임
> 보통은 시분할 시스템 = 멀티 태스킹

#### 멀티 프로그래밍
  - 최대한 CPU를 많이 활용하도록 하는 시스템 
  - 시간대비 CPU 활용도를 높이는 방식
  - 응용 프로그램이 파일을 로드하는 등의 시간(Wait)을 활용
  - 응용 프로그램을 짧은 시간 안에 실행 완료를 시킬 수 있음

#### 멀티 태스킹
  - 단일 CPU에서 여러 응용 프로그램을 동시에 실행하는 것처럼 보이게 하는 시스템

#### 멀티 프로세싱
  - 여러 CPU에 하나의 프로그램을 병렬로 실행
  - 실행속도를 극대화시키는 시스템

## process scheduling

- Queue
  - Job Queue: 현재 시스템 내에 있는 모든 프로세스의 집합
  - Ready Queue: 그중 실행되기를 기다리는 프로세스의 집합
  - Device Queue: Device I/O 작업을 대기하고 있는 프로세스의 집합

- 스케줄러
  <details style='margin-bottom:8px;'>
  <summary>
    장기 스케줄러 (Long-term scheduler / job scheduler)
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>많은 프로세스들이 메모리에 올라올 경우 하드디스크에 임시 저장</li>
    <li>어떤 프로세스에 메모리를 할당하여 Ready Queue로 보낼지 결정</li>
    <li>프로세스에 메모리 및 각종 리소스를 할당 (admit)</li>
    <li>degree of Multiprogramming (DOM) 제어 (실행중인 프로세스의 수 제어)</li>
    <li>프로세스의 상태: new -> ready (메모리 할당)</li>
  </ul>
  </details>

  <details style='margin-bottom:8px;'>
  <summary>
    단기 스케줄러 (Short-term scheduler / CPU scheduler)
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>CPU와 메모리 사이의 스케줄링을 담당</li>
    <li>Ready Queue중 어떤 프로세스를 running 시킬지 결정</li>
    <li>프로세스에 CPU를 할당 (scheduler dispatch)</li>
    <li>프로세스의 상태: Ready -> Running -> Waiting -> Ready</li>
    <li>100ms마다 수행</li>
  </ul>
  </details>

  <details style='margin-bottom:8px;'>
  <summary>
    중기 스케줄러 (Medium-term scheduler / Swapper)
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>여유 공간을 위해 프로세스를 통째로 메모리에서 디스크 swapping (Swap out)</li>
    <li>프로세스에게서 메모리 할당을 해제 (deallocate)</li>
    <li>degree of Multiprogramming (DOM) 제어</li>
    <li>메모리에 너무 많은 프로그램이 동시에 올라가는 것을 조절</li>
    <li>프로세스의 상태: Ready -> Suspended</li>
  </ul>
  </details>

## 비선점형과 선점형 스케줄링

> 스케줄링 적용 시점에 따라 비선점형과 선점형으로 구분된다

### 비선점형 스케줄링 (Non-preemptive Scheduling)
> 어떤 프로세스가 CPU를 할당받으면 그 프로세스가 종료되거나 입출력 요구가 발생하여  
> 자발적으로 중지될 때까지 계속 실행되도록 보장한다  
> 순서대로 처리되는 공정성이 있고 다음에 처리해야 할 프로세스와 관계없이 응답시간을 예상할 수 있다  
> 선점 방식보다 스케줄러 호출 빈도도 낮고 문맥 교환에 의한 오버헤드가 적다  
> 일괄 처리 시스템에 적합하며, CPU 사용시간이 긴 하나의 프로세스가  
> CPU 사용 시간이 짧은 여러 프로세스를 오랫동안 대기시킬 수 있으므로 처리율이 떨어질 수 있다는 단점이 있다.

- FCFS 스케줄링 (First Come First Served Scheduling)
- SJF 스케줄링 (Shortest Job First Scheduling)
- HRRN 스케줄링 (Highest Response Ratio Next Scheduling)

### 선점형 스케줄링 (Premptive Scheduling)
> 어떤 프로세스가 CPU를 할당받아 실행 중에 있어도 다른 프로세스가  
> 실행중인 프로세스를 중지하고 CPU를 강제로 점유할 수 있다  
> 모든 프로세스에게 CPU 사용시간을 동일하게 부여할 수 있다  
> 빠른 응답시간을 요하는 대화식 시분할 시스템에 적합하여 긴급한 프로세서를 제어할 수 있다  
> 운영체제가 프로세서 자원을 선점하고 있다가 각 프로세스의 요청이 있을 때  
> 특정 요건들을 기준으로 자원을 배분하는 방식이다


- RR 스케줄링 (Round Robin Scheduling)
- SRTF 스케줄링 (Shortest Remaining-Time First Scheduling)
- 다단계 큐 스케줄링 (Multileve Queue Scheduling)
- 다단계 피드백 큐 스케줄링 (Multilevel Feedback Queue Scheduling)
- RM 스케줄링 (Rate Monotonic Scheduling)
- EDF 스케줄링 (Earliest Deadline First Scheduling)

## CPU 스케줄러

### FIFO 스케쥴러

- 가장 간단한 스케쥴러 (배치 처리 시스템)
- FCFS (First Come First Served) 스케쥴러

### SJF(Shortest Job First) 스케쥴러

- 최단 작업 우선 스케쥴러
- 가장 실행시간이 짧은 프로세스부터 실행을 시킨다

### Priority‑Based 스케쥴러

- 우선순위 기반 스케쥴러
  - 정적 우선순위 : 프로세스마다 우선순위를 미리 지정
  - 동적 우선순위 : 스케쥴러가 상황에 따라 우선순위를 동적으로 변경

### Round Robin 스케쥴러

- 시분할 시스템 기반


## 프로세스 상태간 관계

- ready
  - ready => running
  - running => ready
  - block => ready

- running
  - running => ready
  - running => block
  - ready => running

- block
  - block => ready
  - running => block

## 용어 zip

- process
  - 메모리에 올려져 실행 중인 프로그램(= task, job)  
  - 응용 프로그램은 여러 개의 프로세스로 구성될 수 있다  
  - 코드 이미지(바이너리): 실행 파일 예:ELF format

- RealTime OS(RTOS)  
  > 응용 프로그램 실시간 성능 보장을 목표로 하는 OS
  > 정확하게 프로그램 시작, 완료 시간을 보장
  > Hardware RTOS, Software RTOS

- General Purpose OS(GPOS)
  > 프로세스 실행시간에 민감하지 않음
  > 일반적인 목적으로 사용되는 OS (Windows, Linux등)

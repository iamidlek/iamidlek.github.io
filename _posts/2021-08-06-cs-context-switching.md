---
title: cs-Context Switching
author: YHole
date: 2021-08-06 +0900
categories: [CS, Operating System]
tags: [cs, Operating System]
---

## Context Switching

- 기본 동작  

어떠한 프로세스가 CPU를 점유(excuting) 하고 있을 때  
다른 대기 중(idle)인 프로세스가 CPU를 점유  
작업 중이던 프로세스는 작업 중이던 내용을 PCB에 저장한다

- 오버헤드  

사실 점유에서 대기, 대기에서 점유가 일어나는 사이에  
PCB에 저장, 가져오기 시간 동안 CPU가 놀게 된다  
컨텍스트 스위칭이 너무 잦으면 성능이 저하된다

- 스레드와 프로세스

PCB는 프로세스의 시작과 함께 생성되는 하나의 block인데  
스레드의 경우 한 프로세스 내에서 text,data,heap을 공유하므로  
스택 및 간단한 정보만 저장하므로  
Process Context Switching 보다 훨씬 빠르다

- 인터럽트

다음과 같은 상황에 컨텍스트 스위칭이 일어난다  
주체는 스케줄러  
- I/O interrupt
- CPU 사용시간 만료
- 자식 프로세스 Fork

등등

## 인터럽트

CPU가 프로그램을 실행하고 있을 때  
예외 상황이 필요한 경우에 CPU 알려서 처리하는 기술  
일종의 이벤트에 해당한다

- 예외 상황 핸들링
  - 예외 발생을 운영체제에 알리기
- 선점형 스케줄러 구현 
  - 스케줄에 맞게 현 프로세스를 중단, 다른 프로세스로 교체
- I/O device
  - 저장메체에서 데이터 처리 완료시 block => ready

![interrupt](/assets/img/docsitem/interrupt.png)

### 주요 인터럽트

내부

- Divide-by-Zero Interrupt
- 사용자 모드의 권한 밖의 명령, 공간 접근시

외부

- Timer Interrupt(선점형 스케줄러)
- I/O Interrupt(저장매체, 키보드, 마우스)
- 전원 이상

### 시스템 콜 인터럽트

- 시스템 콜 실제 코드

```bash
# eax 레지스터에 시스템 골 번호 넣기
mov eax, 1
# ebx 레지스터에 시스템 콜에 해당하는 인자값 넣기
mov ebx, 0
# 소프트웨어 인터럽트 명령을 호출하면서 0x80값을 넘기기
int 0x80
```

1. CPU는 사용자 모드를 커널 모드로 변경
2. IDT를 확인 Ox80에 해당하는 주소(함수)를 찾아서 실행함  
IDT에는 Ox80 => system_call() 의 정보가 기록되어 있다
3. system_call() 함수에서 eax 로부터 시스템 콜 번호를 찾아서  
해당 번호에 맞는 시스템 콜 함수로 이동
4. 해당 시스템 콜 함수 실행 후, 커널 모드에서 사용자 모드로 변경  
다시 해당 프로세스 다음 코드 진행


### IDT

- 인터럽트는 미리 정의되어 각 번호와  
실행 코드를 가리키는 주소가 기록되어 있다
  > 컴퓨터 부팅시 운영체제가 기록 (운영체제 내부 코드)
  > IDT(lnterrupt Descriptor Table)에 기록

- 리눅스의 경우
  - 0 ~ 31 : 예외 상황 인터럽트(일부는 정의 안된 채로 남겨져 있다)
  - 32 ~ 37 : 하드웨어 인터럽트(주변장치 종류, 개수에 따라 변경)
  - 128 : 시스템 콜

## 프로세스의 구성

- Stack : 임시 데이터(함수 호출, 로컬 변수 등)
- Heap : 코드에서 동적으로 만들어지는 데이터
- Data : 변수/ 초기화된 데이터
- Text(code) : 코드

- PC(Program Counter) + SP(Stack Pointer)


### PCB(process control(context) block)

> 프로세스가 실행중인 상태를 캡쳐, 구조화하여 저장

1. Porcess ID
2. Register 값 (PC, SP 등)
3. Scheduling Info (Process State)
4. Memory Info (size limit)  
등등

### 흐름

1. 중지될 때 해당 프로세스의 PCB 정보 업데이트, 메인 메모리 저장
2. 다음 실행 프로세스의 정보(메인 메모리)를 가져온다
3. 가져온 프로세스의 PCB 정보를 CPU의 레지스터에 넣고 실행

> 리눅스의 경우 컨텍스트 스위칭 코드는 각 CPU마다 별도로 존재
> 어셈블리어로 작성할 경우 속도가 빠르므로

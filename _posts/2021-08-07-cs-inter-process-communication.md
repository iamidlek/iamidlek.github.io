---
title: cs-Inter Process Communication
author: YHole
date: 2021-08-07 +0900
categories: [CS, Operating System]
tags: [cs, Operating System]
---

## IPC(Inter Process Communication)

프로세스 간 공간이 완전히 분리되어 있다  
여러 프로세스를 동시 실행하고 정보를 주고받아야 하는  
복잡한 프로그램에선 프로세스 간 통신이 필요하다

> 프로세스를 fork()하여, 여러 프로세스를 동시에 실행  
  > 웹 서버의 예  
  > 새로운 사용자 요청마다 fork() 함수로 새로운 프로세스 생성  
  > 각 사용자 요청에 즉시 대응  
  > CPU 병렬 처리가 된다면 더 빠른 대응  
  > 각 프로세스 제어 및 상태 정보 교환을 위해 프로세스간 통신 필요

## IPC 기법

- file
- Message Queue
- Shared Memory
- Pipe
- Signal
- Semaphore
- Socket  
등등

> file을 제외한 방법은 모두 커널 공간 사용법


### Message Queue

- FIFO 정책으로 데이터 전송
- 서로의 방향으로 큐를 만들면 양방향 가능
- 타이밍은 고민해야할 문제

```c
// A 프로세스 (보내는 쪽)
msqid = msgget(key, msgflg)
// key 1234, msgflg 옵션
msgsnd(msgid, &sbuf, buf_length, IPC_NOWAIT)

// B 프로세스 (받는 쪽)
msqid = msgget(key, msgflg) 
// key 1234 로 해야 해당 큐의 msgid를 얻을 수 있다
msgrcv(msgid, &rbuf, MSGSZ, 1, 0)
```

### Shared Memory

- 노골적으로 Kernel space에 메모리 공간을 만든다
- 해당 공간을 변수처럼 사용
- 공유 메모리 key를 가지고, 여러 프로세스 접근 가능

```c
// 공유 메모리 생성 및 공유 메모리 주소 얻기
shmid = shmget((key_t)1234, SIZE, IPC_CREAT|0666))
shmaddr = shmat(shmid, (void *)0, 0)

// 공유 메모리에 쓰기
strcpy((char *)shmaddr, "Linux Programming")
// 공유 메모리에서 읽기
printf("%s\n", (char *)shmaddr)
```


### Pipe

- 부모 → 자식간의 통신, fork()로 자식 만들어 사용
- 기본 단방향 통신

```c
char* msg = "Hello Child Process!";
int main()
{
char buf[255];
int fd[2], pid, nbytes;
if (pipe(fd) < 0) // pipe(fd)
exit(1);
pid = fork(); // /
if (pid > 0) { // pid ID
write(fd[1], msg, MSGSIZE); //fd[1] .
exit(0);
}
else { // pid 0
nbytes = read(fd[0], buf, MSGSIZE); // fd[0]
printf("%d %s\n", nbytes, buf);
exit(0);
}
return 0;
}
```

![pipe](/assets/img/docsitem/pipe.jpg)


### Signal

- 유닉스에서 30년 이상 사용된 전통적 기법
- 커널 또는 프로세스에서 다른 프로세스에  
어떤 이벤트가 발생되었는지 알려주는 기법
- 프로세스 관련 코드에 관련 시그널 핸들러를 등록, 처리 실행
  - 시그널 무시
  - 시그널 블록 (블록이 풀리면 프로세스에 시그널 전달)
  - 등록된 시그널 핸들러로 특정 동작 수행
  - 등록된 시그널 핸들러가 없으면, 커널에서 기본 동작 수행

- PCB에 해당 프로세스가 블록 또는 처리해야하는 시그널 관련 정보 관리

```c
// 시그널 핸들러 등록 및 핸들러 구현
static void signal_handler (int signo) {
printf("Catch SIGINT!\n");
exit (EXIT_SUCCESS);
}
int main (void) {
if (signal (SIGINT, signal_handler) == SIG_ERR) {
printf("Can't catch SIGINT!\n");
exit (EXIT_FAILURE);
}
for (;;)
pause();
return 0;
}

// 시그널 핸들러 무시 
int main (void) {
if (signal (SIGINT, SIG_IGN) == SIG_ERR) {
printf("Can't catch SIGINT!\n");
exit (EXIT_FAILURE);
}
for (;;)
pause();
return 0;
}
```


#### 주요 시그널

-	SIGKILL: 프로세스 죽이기  
(슈퍼관리자가 사용하는 시그널로, 프로세스는 어떤 경우든 죽도록 되어 있음)
-	SIGALARM: 알람을 발생한다
-	SIGSTP: 프로세스를 멈춰라(Ctrl + z)
-	SIGCONT: 멈춰진 프로세스를 실행해라
-	SIGINT: 프로세스에 인터럽트를 보내서 프로세스를 죽여라(Ctrl + c)
-	SIGSEGV: 프로세스가 다른 메모리영역을 침범했댜

### Socket  

- 소켓은 네트워크 통신을 위한 기술
- 기본적으론 클라이언트와 서버등 두개의 다른  
컴퓨터간의 네트워크 기반 통신을 위한 기술

- IPC의 경우  
하나의 컴퓨터 안에서 두개의 프로세스간의 통신으로 사용


### Semaphore

다음 포스트에서 Thread(스래드)와 함께 알아보자

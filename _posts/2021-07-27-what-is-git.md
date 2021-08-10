---
title: Git 이란?
author: YHole
date: 2021-07-27 +0900
categories: [프론트 엔드, Git]
tags: [Git, Github]
---

## 간략한 역사

- 1965년 데니스 리치, 켄 톰슨 외 AT&T Bell 연구소에서  
  PDP-7 기반 어셈블리어로 작성한 UNIX를 개발
- 1973년 데니스 리치와 켄 톰슨이 C언어 개발 후, C 기반 UNIX 재작성
- 1984년 리처드 스톨만이 오픈 소프트웨어 자유성 확보를 위한 GNU 프로젝트 돌입  
  GNU == G NU is N ot U nix
- GNU에는 Kernel 이 존재하지 않았고,  
  Linux Kernal을 만들기 위해 Subversion을 쓰다 화가 난 리누스 토발즈는  
  2주 만에 git이라는 버전 관리 시스템을 만들게 된다.

### 용어 정리

<details style='margin-bottom:8px;'>
  <summary>
    Linux
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>리누스 토발즈가 작성한 커널</li>
    <li>GNU 프로젝트의 라이브러리와 도구가 포함된 운영체제</li>
    <li>PC와 모바일, 서버, 임베디드 시스템 등 다양한 분야에서 활용</li>
    <li>다양한 배포판이 존재 Redhat, Debian, Ubuntu, Android</li>
  </ul>
</details>

<details style='margin-bottom:8px;'>
  <summary>
    Shell
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>운영체제의 커널과 사용자를 이어주는 소프트웨어 (유닉스 쉘)</li>
    <li>sh(Bourne Shell) : AT&T Bell 연구소의 Steve Bourne이 작성</li>
    <li>csh : 버클리의 Bill Joy가 작성</li>
    <li>bash(Bourne Again Shell) : Brian Fox가 작성</li>
    <li><strong>zsh</strong> : Paul Falstad가 작성</li>
    <p>ㅤㅤ→  sh 확장형, 현재 가장 완벽한 쉘</p>
  </ul>
</details>

<details style='margin-bottom:8px;'>
  <summary>
    Cloud Remote Repository Services
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>Github : 비영리에서 Microsoft에 인수, 가장 유명한 서비스</li>
    <li>Bitbucket : Atlassian이 서비스</li>
    <li>ㅤㅤㅤㅤ→ jira, confluence, trello 등의 부가 도구와 유기적 작업</li>
    <li>GitLab : GitLab이 서비스. 사설 서버 구성이 가능</li>
  </ul>
</details>

## Git

- Version Control System - 분산형 버전 관리 시스템
- 브런치를 통한 같은 파일을 여러명이 개발하는 병렬 개발이 가능
- 로컬 저장소가 있기 때문에 원격 저장소의 복구가 가능
- 인터넷이 연결되어 있지 않아도 개발이 가능
- 사본을 로컬에서 관리하기 때문에 SVN보다 빠름
<details style='margin-bottom:8px;'>
  <summary>
  SVN(SubVersion) 
  </summary>
  <ul style='list-style:none;padding-left:28px;'>
    <li>중앙 서버에 소스 코드와 히스토리를 저장</li>
    <li>서버에 장애가 생기면 모두의 작업이 중단</li>
    <li>클라이언트 - 서버  와 같은 모델</li>
  </ul>
</details>

### 기본 용어

- Repository : 저장소는 히스토리, 태그, 소스의 branch에 따라 버전을 저장한다  
                     저장소를 통해 작업자가 변경한 모든 히스토리를 확인 할 수 있다  
                     로컬 저장소(Local Repository)와 원격 저장소(Remote Repository)가 있다  
- Working Directory : 로컬 작업 폴더를 의미하며 필요한 파일들을 수정하거나 생성한다
- Staging Area : add 명령어로 commit을 하기위한 파일들을 올려 놓는 곳이다.
- Head : 현재 본인이 작업중인 Branch를 가리킨다.
- Branch : 분기점을 의미하며, 작업을 할때에 현재 상태를 복사하여  
               새로운 Branch에서 작업을 한 후에 Merge를 한다.
- Modified : 로컬에서 작업 후 데이터의 내용이 변경된 것을 의미한다.
- Staged : Modified 된 파일이 스테이지 영역에 있음을 의미한다.
- Committed : 로컬 저장소에 저장되었음을 의미한다.
- Snapshot : Commit 하면 변경된 사항들이 스냅샷으로 저장됨 -m 은 변경 내용에 대한 주석

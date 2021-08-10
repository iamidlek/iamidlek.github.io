---
title: Git flow
author: YHole
date: 2021-08-02 +0900
categories: [프론트 엔드, Git]
tags: [Git, Github, git-flow]
---

## Git flow




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

### 기본 명령어

- git init : 위치한 폴더의 깃 저장소를 초기화한다.  
              .git 폴더가 생성되며 파일 추적, git 관련 작업을 할 수 있게된다.
- git remote :  뒤에 add 원격 저장소 이름(origin) 원격 저장소 주소  로 로컬과 연결한다.
- git help :  21개의 가장 많이 사용하는 깃 명령어들이 나타난다.  
               사용법에 대해 자세하게 알고싶으면 “git help init”처럼 뒤에 명령어를 붙여준다.
- git config : 뒤에 —global user.name 처럼 전역 설정, 초기 설정이 가능하다.
- git status : 저장소 상태를 체크한다. 브랜치는 어디로 설정되어 있는지,   
                  commit 된 것이나, working directory의 상태를 보여준다.
- git clone : 원격 저장소의 저장소를 로컬에서 이용할 수 있게 그대로 복사해 가져온다.
- git add : git 이 파일들을 추적한다. 파일을 추가하면, staging area에 포함된다.
- git commit : staging area의 내용들을 메세지와 함께 로컬 저장소에 저장한다.
- git push : 로컬 저장소에서 원격 저장소로 업로드가 이루어진다.  
                git push [원격 저장소 이름] [원격 저장소 푸쉬 할 브랜치]  
                로컬에만 있는 프렌치의 경우 -u 옵션이 초회에 필요하다.
- git pull : 원격 저장소에서 로컬 저장소로 다운로드 후 병합이 이루어진다.  
               git pull [원격 저장소 이름] [원격 저장소 브랜치]
- git fetch : 원격 저장소의 변경사항을 다운로드 한다. 병합을 신중히 해야 하는 경우 사용한다.  
                로컬의 브랜치는 최근 커밋 위치를 원격은 가져온 최신 커밋 위치를 가르킨다.  
                git diff HEAD origin/main 원래 내용과의 차이 확인  
                git log —decorate —all —oneline 커밋이 얼마나 되었는지 확인 후 병합
- git merge : 현재 브런치에 병합할 브런치명을 작성한다.
- git log : 커밋 내역, 버전 생성 확인을 위한 명령어이다.
- git branch : 현재 브랜치를 확인하거나 뒤에 브랜치명을 붙여 생성도 할 수있다.  
                    -v (commit ID), -r (원격 브랜치), -a(모든 브랜치), -d(브랜치 삭제)
- git switch : 브랜치를 바꾸는 명령어 upstream/변경한 브랜치 가 된다.  
                    upstream - 원천, 상류 로컬에서 원격으로 push 하는 개념
- git restore : staging area 에 들어간(add된) 것을 되돌릴때 사용한다.  
                    git restore —staged 파일명
- git diff : Unstaged 된 파일을 비교 diff —git a/test.txt b/test.txt  
              —staged 또는 브랜치명 브랜치명 등 비교할 때 사용된다.

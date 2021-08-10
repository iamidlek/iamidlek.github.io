---
title: Git flow
author: YHole
date: 2021-08-02 +0900
categories: [프론트 엔드, Git]
tags: [Git, Github, git-flow]
---

# Git flow

효과적인 브렌치 사용을 위하여  

**[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)** by Vincent Driessen

[설치 및 사용법](https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html)

## 전략

### 5개의 브랜치

- main : 배포용 브랜치
- develop : 개발을 진행하는 브랜치
- feature : develop에서 분기하는 기능 단위별 브랜치
- hotfixs : 긴급한 트러블슈팅이 진행되는 브랜치
- release : 배포 가능한 상태의 소스가 저장되는 브랜치

### 흐름

 1. 초기화

```bash
git flow init
```

 2. develop 브랜치에서 기능 가지 생성

```bash
# 브랜치 생성과 이동이 일어난다
git flow feature start 브랜치명

# 원격 저장소에 브랜치를 올립니다
git flow feature publish 브랜치명

# 원격의 브랜치를 가져옵니다
git flow feature pull origin 브랜치명

# 브랜치 머지,삭제 develop으로 돌아온다
git flow feature finish 브랜치명
```

 3. release 하기 (main에 영향)

```bash
# 브랜치명 위치에 develop브랜치의 commit해시를 입력할 수도 있다
git flow release start 버전명

# 원격 저장소에 브랜치를 올립니다
git flow release publish 버전명

# 원격 저장소에서 가져오옵니다
git flow release track 버전명

# main 브랜치에 병합, release 버전을 태그로 생성
# 또한 develop 브랜치와 병합이루어지고
# release 브랜치를 삭제한다
git flow relese finish 버전명
```

 4. push 하기

```bash
# main, develop 둘다 원격 저장소에 올린다
git push

# 태그도 올려준다
git push --tags
```

## 역할에 따른 flow

- 메인을 관리하는 [팀장]
- 개발을 진행하는 [팀원]

#### [팀장]

1. main과 develop을 관리한다
2. 기본 구성이 끝나면 팀원에게 fork를 요청한다
3. develop에 직접 혹은 feature branch에 개발을 한다
4. 각 팀원들의 pr을 develop에 합친다
5. git fetch origin develop & merge FETCH_HEAD로 조정 병합
6. 배포 준비가되면 release브랜치로 버전을 만든다
7. release를 진행하고 tag와 각 브랜치를 원격에 push한다

#### [팀원]

1. 원본이 되는 repo에서 fork 해온다
2. (clone 과 다르게 원본과 연결되어있어 추가기능 넣기가 가능)
3. 로컬에서 git remote -v 로 확인
4. git remote add upstream 팀장git주소 로 연결
5. git fetch upstream develop & merge FETCH_HEAD
6. feature branch에 개발을 한다
7. 원본 develop브랜치에 pr을 보낸다


## Github Projects&Issues 관리

Github 레포지토리에가면 프로젝트와 이슈 탭이 존재한다

- 팀장이 Projects를 관리
  - 프로젝트 생성과 프로젝트 컬럼, 아이템 관리
  - 마일스톤과 기일 관리
- 팀원이 이슈에 자신의 작업, 일정 등을 올림
- 팀장이 이슈를 프로젝트에 옮기거나 관리한다

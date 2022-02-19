---
title: Git 초기 설정
author: YHole
date: 2022-02-19 +0900
categories: [Front end, Git]
tags: [Git]
---

# Git 설정

## 전역(global) 설정

### 기본 설정

```bash
git config --global user.name "Github아이디"
git config --global user.email "이메일"
git config --global core.editor "vim"
git config --global core.pager "cat"
git config --global init.defaultBranch main
```

```bash
# 설정 취소
git config --global --unset user.name
```

### 편집기 설정

```bash
# vscode로 수정하기 설정
git config --global core.editor "code --wait"
```

```bash
# vscode로 수정하기 취소
# 해당 설정 삭제
[core]
  editor = code --wait
```

### 설정 내용 확인

```bash
# 설정 목록 출력
git config --list
git config --global --list
```

```bash
# 에디터로 보기
git config -e
git config --global -e
```

## 추가 설정

### 줄바꿈

```bash
# 윈도우
git config --global core.autocrlf true
# Mac
git config --global core.autocrlf input
```

### pull strategy

- _참고_ pull 전략에 대한 자세한 내용은 별도 포스팅

```bash
# merge (the default strategy)
# 머지 커밋 생성됨
git config pull.rebase false
# fast-forward only
# 빨리 감기로 머지 커밋이 생성 되지 않음
git config pull.ff only
# rebase
# 브렌치 기록이 남지 않고 main으로 이동
git config pull.rebase true
```

### push

```bash
# push시 local과 동일한 브랜치명으로 올라감
git config --global push.default current
```

### 단축 명령어 생성

```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.cam "commit -am"
git config --global alias.unstage 'reset HEAD --'
```

## 로컬(local) 설정

### git 생성

```bash
# 관리할 root 폴더에 .git 생성
git init
```

### 원격 설정

```bash
# 원격 주소 설정
git remote origin http...com
```

### 로컬 설정

- 글로벌 정정에서 --global없이 사용하면 로컬 설정 가능

---
title: git-fixDate
author: YHole
date: 2021-09-10 +0900
categories: [Front end, Git]
tags: [Git, fix date, commit, rebase]
---

## rebase

- rebase를 이용한 커밋 일자 변경

- 먼저 log를 살펴 본다
  
  ```bash
  git log
  ```

- 수정할 commit의 [이전 commit]의 해쉬 값을 복사  

  ```bash
  commit 2189d0ecf44c5146ed7c5ec726f83da97c96285f
  ```

- 명령창에 이하와 같이 입력한다

  ```
  git rebase 복사한해시값 -i
  ```

- edit 부분에 변경할 commit을 입력한다

```
git commit --amend --no-edit --date "Aug 23 01:00:00 2021 +0000"
```

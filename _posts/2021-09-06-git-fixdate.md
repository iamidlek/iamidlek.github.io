---
title: git-fixDate
author: YHole
date: 2021-09-06 +0900
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

  ```bash
  git rebase 복사한해시값 -i
  ```

- edit 부분에 변경할 commit을 입력한다

  ```bash
  edit 8b110cbb535078176ea9031b1f94059b870bcaa7
  pick 805e6cf 
  ```

- 변경할 날짜를 지정한다

  ```bash
  git commit --amend --no-edit --date "Aug 23 01:00:00 2021 +0000"
  ```

- rebase continue 해준다

  ```bash
  git rebase --continue
  ```

### 메모

- 향후에 rebase를 조금 더 깊게 공부하고 싶다
- 실제로 어떤 commit 그래프가 나오는지 vs code 확장인  
git graph로 확인하며 공부하면 좋을 것 같다

---
title: Git 병합하기
author: YHole
date: 2022-02-21 +0900
categories: [Front end, Git]
tags: [Git]
---

# merge와 rebase

- merge : branch기록이 남으며 main에서 진행
- rebase : branch기록이 남지 않으며 해당 branch에서 진행 후 merge

## merge

```bash
# main에 위치
git merge 병합브렌치명

# 합쳐진 브렌치는 삭제
git branch -d 브렌치명

# 되돌리기 가능
git reset --hard
```

## rebase

```bash
# 해당 브렌치에 위치
git rebase main

# main으로 이동 후
# fast-forward(빨리 감기)한다
git merge 브렌치명

# 합쳐진 브렌치는 삭제
git branch -d 브렌치명
```

## 충돌 시

- 충돌 부분을 확인 후 수정

```bash
# merge
# 충돌을 해결 한 후
git add .
git commit
```

```bash
# rebase
# 충돌을 해결 한 후
git add .
git rebase --continue
git switch main
git merge 브렌치명
```

- 취소(중단) 방법

```bash
# merge
git merge --abort

# rebase
git rebase --abort
```

---
title: Git push 충돌해결
author: YHole
date: 2022-02-22 +0900
categories: [Front end, Git]
tags: [Git]
---

# push와 pull

## push 충돌 시

- github이 local보다 더 최신의 상태일 경우 pull이 선행 되어야 한다.

```bash
# github
a => b => c => e

# local
a => b => c => d

# local에 e가 없기 때문에 문제가 발생
```

### merge 방식

- 변경 사항을 merge한 커밋을 생성하여 합침
- github의 e와 local의 d를 합친 f커밋이 생성

```bash
git pull --no-rebase
            => e =>
a => b => c => d => f
```

### rebase 방식

- github의 e를 붙이고 local의 d를 그 후에 붙여 준다
- _pull 상황에서의 rebase는 사용해도 좋다_

```bash
git pull --rebase

a => b => c => e => d
```

### 기타

- 강제로 local상태를 원격에 보내기
- **원격의 커밋내용이 local의 내용으로 대체되므로 사용시에는 주의가 필요**

```bash
git push --force
```

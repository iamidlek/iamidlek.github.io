---
title: team project-bankapp-6
author: YHole
date: 2021-09-19 +0900
categories: [Front end, practical exercise]
tags: [team project, bankapp, practical]
---

## reduce 활용

- 객체에 key 값이 없으면 추가
- key 값이 들어오면 금액을 더해줌
- 즉, key 값에 해당하는 금액들이 합산됨

  ```js
  const accum = logs.reduce( function (obj,item) {
      if (!obj[item.classify]){
        obj[item.classify] = 0
      }
      obj[item.classify] += item.price
      return obj
    }, {})
  ```

### 추가 예

```js
const data = ['car', 'car', 'truck', 'truck', 'bike', 
'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ]

const eight = data.reduce(function(a,b){
  if (!a[b]) {
    a[b] = 0;
  }
  a[b]++;
  return a
}, {});
```

### 메모

익숙하지 않으면 사용하기 힘든 부분도 있어서  
자주 사용해 보도록 해야 좋을 것 같다

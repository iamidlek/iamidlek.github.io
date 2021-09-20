---
title: team project-bankapp-4
author: YHole
date: 2021-09-17 +0900
categories: [Front end, practical exercise]
tags: [team project, bankapp, practical]
---

## 수평 스크롤

  ```js
  const horizontals = document.querySelectorAll('.saving')

  horizontals.forEach( horizontal => {
  horizontal.addEventListener('wheel', e => {
    e.preventDefault();
    if(e.wheelDelta > 0){
      // scroll up -> move left
      horizontal.scrollLeft -= 20;
    }else{
      // scroll down -> move right
      horizontal.scrollLeft += 20;
		}
    })
  })
  ```

### 메모

MDN 문서를 참고하여 구현한 코드  
MDN에 역시 참고할 사항이 많은 것 같다

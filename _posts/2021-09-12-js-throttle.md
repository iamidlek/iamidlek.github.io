---
title: js-throttle
author: YHole
date: 2021-09-12 +0900
categories: [Front end, js]
tags: [js, throttle, window, scroll]
---

## throttle

- 의미  
실행된 후 마무리가 될 때까지 재실행되지 않음

- 예시  
실행시간이 500ms일 경우 실행중 재실행 요청이 와도  
다시 실행되는 것이 아니라 무시됨  
마무리 된 후에 요청이 오는 경우 새로 실행됨

- 사용하는 이유
스크롤같은 경우 너무 빈번하게 이벤트가 발생하므로  
일정 interval을 두고 실행을 시키기 위함
슬라이드 같은 경우 요소가 이동중 또 한번의 이벤트를  
실행 시키지 않기 위함(완전히 다음 슬라이드로 이동한 후  
다시 페이지 넘기는 이벤트를 받는다)

### 구현 코드

- 핵심 코드는 이하와 같다

```js
let timer;
el.addEventListener('click',
function(e) {
  // 초기 timer => undefined 조건문 무조건 실행
  if (!timer) {
    // timer = 1 인 상태(true)
    timer = setTimeout(function() {
    console.log('실행 내용');
    // 다시 if 조건에 걸리도록 바꾸어 준다
    timer = null;
    }, 500);
  }
});
```

### 메모

상황에 따라서 Debounce 또는 Throttle 을 사용하여  
 이벤트 발생 빈도를 줄인다.

debounce는 연이은 이벤트중 마지막 부분을 실행  
throttle은 이벤트 시작과 끝 사이의 이벤트들을 무시  
첫 이벤트가 끝난 후에 발생한 이벤트를 실행한다.

즉 이벤트가 연속해서 발생하는 상황이라면  
debounce는 이벤트의 초기화만 계속 일어나고 결과 없음
throttle은 시작과 끝에 해당하는 시간마다 결과가 나오고  
그 사이에 발생한 이벤트는 실행 없이 무시된다

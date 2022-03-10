---
title: js-debounce
author: YHole
date: 2021-09-11 +0900
categories: [Front end, js]
tags: [js, debounce, window, scroll]
---

## debounce

- 의미  
  연이은 요청을 그룹화 하여 마지막 요청만 + 지연시간 후 처리

- 예시  
  요청에 대한 지연시간 200ms을 주고 지연시간 내에 요청이 오면  
  새 요청이 기존요청을 대체하고 남은 지연시간도 200ms로 초기화

- 사용하는 이유
  서버 요청과 같은 경우 비용에 해당한다.  
  유저가 어떤 값을 입력하는 순간순간마다 요청을 보내면 비용이 상승  
  어느정도 입력하는 시간을 기다린후 완료된 키워드로 요청을 보내면  
  비용도 절감하고 사용자도 완료되지 않은 키워드의 답을 보지 않아도 된다

### 구현 코드

- 핵심 코드는 이하와 같다

```js
let timer;
el.addEventListener("input", function (e) {
  // 200ms 중이 아니라면 조건문에 걸리지 않음
  if (timer) {
    clearTimeout(timer); // 200 까지 세는걸 0 으로 돌림
  }
  // 지연시간 뒤 실행될 내용 timer에는 1 이 반환된다
  // 지연시간 중 이벤트가 다시 발생하면 if 문에 걸린다
  timer = setTimeout(function () {
    console.log("입력 받은 내용", e.target.value);
  }, 200);
});
```

- 다른 function과 함께 사용하기

```js
function debounce(func, wait = 20, immediate = true) {
  var timeout;
  return function () {
    // 이미 존재하는 함수를 호출할 때
    var context = this;
    // 이는 호출된 객체의 지정되지 않은 모든 인수에 대해 사용
    var args = arguments;
    //later라는 함수 만들기
    var later = function () {
      timeout = null;
      // immediate는 true 가 기본으로 들어오므로 실행되지 않음
      if (!immediate) func.apply(context, args);
    };
    var callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
    // func 가져다 사용할 메소드
    // context 현재 객체로 사용될 객체
    // args 함수에 전달될 인수 집합
    if (callNow) func.apply(context, args);
  };
}
```

### 원리

- 함수를 반환하여 somefunc를 함수로 이용

```javascript
const debounce = () => {
  return () => "it`s me";
};

const somefunc = debounce();

somefunc();
// "it`s me"
```

- 값을 받아 넘주기가 가능

```javascript
const debounce = () => {
  return (...args) => console.log(args);
  // or
  // return function (...args) { console.log(args) }
};

const somefunc = debounce();

somefunc("content1", "content2");
// [ 'content1', 'content2' ]
```

- 넘긴 값을 이용하는 함수를 넘기기

```javascript
const debounce = (func) => {
  return (...args) => func.apply(null, args);
  // return (...args) => func.apply(this, args);
  // or
  // return function (...args) { func.apply(null, args) }
  // return function () { func.apply(null, arguments) }
};

// args가 넘어오는 함수
const func = (a, b) => console.log(a, b);

const somefunc = debounce(func);

somefunc("content1", "content2");
// content1 content2
```

- 시간을 넘겨 debounce 완성하기

```javascript
const debounce = (func, time) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(null, args);
    }, time);
  };
};

const func = (a, b) => console.log(a, b);

const somefunc = debounce(func, 3000);

somefunc("content1", "content2");
// after 3000ms
// content1 content2
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

---
title: js-copy-object
author: YHole
date: 2021-09-09 +0900
categories: [Front end, js]
tags: [js, copy, shallow, deep, object]
---

## 복사

> 단순복사는 완전히 동일한 주소  

> 얕은복사(shallow copy)는 껍데기만 다른 주소  
  내용은 동일한 주소  

> 깊은복사(deep copy)는 재귀적 복사로 전부 다른 주소

### 객체

```js
const cli = {
      name: 'yoo',
      age: 20
    };
```

```js
const copy = cli;

copy.age = 0;
// 원본 cli 객체의 값도 변화한다
```

- 얕은 복사가 일어나는 경우
  - 내부에 참조 객체가 없다면 깊은 복사

```js
const copy2 = Object.assign({}, cli);
copy2.number = 10
copy2.age = 12
// 원본에 없는 number : 10 을 추가 할 수 있다
// 원본에 영향을 주지 않는다

const copy2 = Object.assign({}, cli, { number: 10, age: 12 });
// 변경 내용을 바로 줄 수도 있다
```

```js
const cli = {
      name: 'yoo',
      like: ['apple','banana']
      social: {
        email: 'abc@abc.com',
        home: 'https://'
      }
    };

// 객체안의 배열과 객체가 있으면 얕은 복사가 된다
const temp = Object.assign({}, cli);
cli.social.email = 'none'

// 원본 객체의 내용도 변경이 된다
```

### 깊은 복사

- 권장되는 방법은 아닌 것 같지만 이하와 같다

> gettser/setter 등 JSON 으로 변경할 수 없는 프로퍼티는 무시된다  
> 배열도 깊은 복사가 가능하지만 배열 관련 함수는 사용 불가능해 진다


```js
// 문자열로 변환 후 완전 새로운 객체를 만들기
const temp2 = JSON.parse(JSON.stringify(cli));
```

- 다른 방법으론 재귀적으로 복사할 반복문 또는 lodash를 이용한다

---
title: Module
author: YHole
date: 2021-09-21 +0900
categories: [Front end, bundler]
tags: [js, bundler, Module]
---

## Module

프로그램을 구성하는 내부 코드가 기능 별로 나뉘어 있는 것

> 모듈을 인식하는 Module System 과 키워드가 필요하다

### 표준

1. CommonJS (Node.js)
2. ESM (ECMAScript 2015)


### 키워드

1. 내보내기
  - 한곳으로 내보내기
  - 개별적으로 내보내기
2. 가져오기


### 사용 예

#### CommonJS

- 하나의 객체로 내보내기

```js
// 모듈화 할 내용

const A = ...
const B = 함수

module.exports = {
  A,
  B
}
```

```js
// 가져오기

// A 만 가져오기
const { A } = require('./경로')
```

- 개별적으로 내보내기

```js
// 각각 내보낸다
exports.A = A
exports.B = B
```

> 내보내기 기능은 한가지로 정해서 일관되게 만들자


#### ESM (ECMAScript 2015)

```bash
npm i esm

# 명령어 바뀜
node -r esm index.js 
```


```js

const A = ...
const B = 함수

export {
  A,
  B
}
```

```js
// 가져오기

import { A } from '경로'
```

> export defalut 로 하면 해당 모듈.함수이름 이런 식으로 이용해야 한다



### module의 종류

1. built-in Core Module (Node.js 기본 모듈)
2. Community-based Module (npm)
3. Local Module (본인이 만든 것)


### module 장점

1. 코드의 재사용성 증가
2. 코드의 관리가 용이

> 다만  모듈화 기준은 명확해야 한다

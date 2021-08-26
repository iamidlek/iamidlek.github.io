---
title: ts-시작하기-1
author: YHole
date: 2021-08-24 +0900
categories: [Front end, ts]
tags: [typescript, ts]
---

## TypeScript

> ts는 js에 타입을 부여한 언어(js의 확장된 언어)  
> ts는 js로 컴파일(complile)이 필요하다
> 컴파일 단계에서 에러 확인이 가능  
> 타입 지정으로 가이드 가능


### 설치

```bash
npm init -y
npm i -D typescript
npx tsc --init
```

- 컴파일

```bash
# js 로 컴파일 js파일 생성
npx tsc

# 실행
node 파일명.js
```

### type annotation(타입 주석)

- js의 기본 자료형 포함(superset)
  - Boolean
  - Number
  - String
  - Null
  - Undefined
  - Symbol
  - Array (object)

- 추가로 제공되는 자료형
  - Any, Void, Never, Unknown
  - Enum
  - Tuple (object)


## Primitive Type

- 이하의 방법은 권장하지 않는다

```js
// typeof 하면 object가 된다
new Boolean(false);
new String('a');
new Number(1); 
```

- 기본 타입에 해당하는 데이터를 객체로 포장해 주는 것을  
래퍼 객체(Wrapper obj)라고 합니다

### Boolean

```ts
let isDone: boolean = false;
isDone = true;

console.log(typeof isDone);
// 'boolean'

// 리터럴값으로 true(프리미티브 값)을 주는게 맞다
let isOk: Boolean = true;

// 안되는 경우 틀린경우
// new Boolean 을 사용하는 경우는 객체가 되어버림
let isNotOk: boolean = new Boolean(true);
```

### Number

```ts
// js와 동일 하게 모든 숫자는 부동 소수점 값
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let notANumber: number = NaN;

// 백만을 표기하는 방법(알어보기쉽게)
// 실제 output에는 영향없음
let underscoreNum: number = 1_000_000;
```

### String

```ts
// 텍스트 형식을 참조하기 위해 '' "" 사용
let myName: string = 'Yoo';
myName = "kim";
// string 재할당가능

// Template String
// 이전엔 + 기호를 사용해서 문자열 + 변수/상수 를 사용
// `${}`

let fullName: string = 'Yoo Kim'
let age: number = 39;

let sentence: string = `Hello, My name is ${ fullName }. 

I'll be ${age + 1}years old next month.`;

console.log(sentence)
//Hello, My name is Yoo Kim. 
//
//I'll be 40years old next month.
```

### Symbol

```ts
// ECMA 2015에서 추가됨
// new Symbol로 사용할 수 없다
// Symbol 을 함수로 사용하여 symbol타입을 만들어낼수있다
// 함수사용시 대문자시작 타입은 소문자

// tsconfig.json 의 내용을 이하와 같이 수정하면
// "lib": ["ES2015","DOM"],  

// 이하의 코드가 에러가 안난다

console.log(Symbol('foo') === Symbol('foo'));
// 같은 프리미티브 값을 받지만 둘은 다르다

// 프리미티브 타입의 값을 담아서 사용
// 고유하고 수정불가능한 값으로 만들어진다
// 접근을 제어하는데 사용

// const obj = {
//   sym: "value",
// };

// obj["sym"]
// 아무나 접근가능

// 이하처럼 [sym] 해주면
// obj[sym]으로만 접근이 가능하다

const sym = Symbol();
// sym 은 고유해진다

const obj = {
  [sym]: "value",
};

obj[sym]
// value
```

- 2번째 포스트에서 이어짐
  - [이어보기]()

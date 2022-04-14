---
title: 함수형 프로그래밍 - map
author: YHole
date: 2022-04-14 +0900
categories: [Front end, js]
tags: [functional]
---

# number의 부재

```typescript
<A,B>(oa: Option<A>, f: (a: A) => B)

Option<B>

// Array의map
(arr: Array<A>, f: (a: A) => B) => Array<B>

// Option의map
(oa: Option<A>, f: (a: A) => B) => Option<B>

// functor의 map
// f에의해내부의값은변경되어도구조는변경되지않는다.
```

- 함수를 인자로 받는 고차 함수

```typescript
                              // 함수를 인자로
const map = <A, B>(oa: Option<A>, f: (a: A) => B){
  if(isNone(oa)) return oa;
  return some(f(oa.value));
}
```

- 함수를 반환하는 고차 함수

```typescript
const add = (a: number) => (b: number) => a + b;
const add3 = add(3);
// (b) => 3 + b 함수를 반환

add3(4); // 3 + 4 = 7
add3(5); // 3 + 5 = 8
```

## 부분 함수

매개변수로 가능한 값들 중에 일부 경우에만 반환값이 있는 함수

## 부분 적용

여러 매개변수를 받는 함수에 일부 인자만 미리 적용해서, 나머지 인자를 받는 함수 만들기

> 부분 함수와 전혀 다르고 커링과 비슷

```typescript
(a:A, b: B, c: C) => D

(a: A, b: B) => (c: C) => D
```

## 커링

여러 매개변수를 받는 함수를 한 개의 인자만 받는단 인자 함수들의 함수열로 만들기

```typescript
(a:A, b:B, c:C) => D

(a: A) => (b: B) => (c: C) => D
```

### Partial Application 부분적용이란?

인자가 여러 개인 함수의 전체 인자 중에 인자 몇 개를 고정하여 더 작은 개수의 인자를 가지는 또 다른 함수를 생성하는 프로세스

```typescript
const delivery = (present: string, from: string, to: string): string => {
  return `보내는물건: ${present}보내는사람:${from}받는사람:${to}`;
};

// 만약 "첵", "어머니"가 반복된다면
console.log(deliver(“책”, “어머니”, “아들”))
console.log(deliver(“책”, “어머니”, “딸”))

const delivery = (present: string, from: string) => (to: string) => {
  return `보내는물건: ${present}보내는사람:${from}받는사람:${to}`;
};

const momsPresent = deliver(“책”, “어머니”);
console.log(momsPresent(“아들”))
console.log(momsPresent(“딸”))
```

### Currying 커링

인자가 여러 개인 함수를 인자가 하나인 함수들의 함수열(sequence of functions)로 만들기

```typescript
// 인자가여러개인함수:
(A, B, C) => D

//커링을적용curry((A, B, C) => D)

curried function: (A) => (B) => (C) => D
```

```typescript
const delivery = (present: string) => (from: string) => (to: string) => {
  return `보내는물건: ${present}보내는사람:${from}받는사람:${to}`;
};

const giveBook = deliver(“책”)
const momsPresent = giveBook(“어머니”);
console.log(momsPresent(“아들”))
console.log(momsPresent(“딸”))

const currying3 = <A, B, C, D>(f: (a: A, b: B, c: C) => D) => (a: A) => (
  b: B
) => (c: C) => f(a, b, c);

const curriedDelivery = currying3(delivery);

export const main = () => {
  console.clear();

  const momsPresent = curriedDelivery("상품권")("엄마");
  console.log(momsPresent("아들"));
};
```

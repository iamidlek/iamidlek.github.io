---
title: 함수형 프로그래밍 - Array 1
author: YHole
date: 2022-04-11 +0900
categories: [Front end, js]
tags: [functional]
---

# 개수를 결정할 수 없는 값들

```typescript
const numbers: Array<number> = [1, 2, 3]; // 3개도가능하고
const strings: Array<string> = [“hello”, “world”]; // 2개도가능하다
```

## 비결정적인 함수

```typescript
type filter = <A>(arr: Array<A>, pred: (x: A) => boolean) => Array<A>;
```

## 고차함수

- 다른 함수를 매개변수로 입력받는 함수

```typescript
type hof1 = (func: 함수, x: 인자) => B

function(func: 함수, x: 인자): B {
  …
  const y = func(x)
  return y
}
```

- 새로운 함수를 반환하는 함수

```typescript
type hof2 = (a: A) => 함수

function(a: A): 함수 {
  …
  return (b: B) => a + b
}
```

### 다른 함수를 매개변수로 입력받는 함수

- array.map(함수)

```typescript
export const map = <A, B>(array: Array<A>, f: (a: A) => B) => {
  const result: Array<B> = [];
  for (const value of array) {
    result.push(f(value));
  }
  return result;
};
```

- array.filter(함수)
- array.reduce(함수)

### 예외를 표현하는 데이터 타입

```typescript
function stringToNumber(s){
  const result = Number(s);
  if (isNaN(result)){
    // 부수효과
    throw Error(s+”: 숫자가아닙니다.”);
  }
  return result;
}

type Try<T> = Error | T (s: string) => Try<number>
```

### Array map의 타입 다시 보기

```typescript
// 제네릭으로 구현한 map
type map<A, B> = (Array<A>, A⇒ B) ⇒ Array<B>
// 타입 파라미터 A가 number 타입이라면?
(Array<number>, number⇒ B) ⇒ Array<B>
```

- 연습

```typescript
export type MapType<A, B> = (xs: Array<A>, f: (x: A) => B) => Array<B>;
// (Array<A>, A => B) => Array<B>

export type MapType1 = MapType<number, number>;
// (Array<number>, number => string) => Array<string>

export type Compose<A, B, C> = (g: (y: B) => C, f: (x: A) => B) => (a: A) => C;
// (B => C, A => B) => A => C

export type Compose1 = Compose<string, number, boolean>;
// (number => boolean, string => number) => string => boolean
```

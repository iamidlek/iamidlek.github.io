---
title: 함수형 프로그래밍 - 함수와 타입 - 2
author: YHole
date: 2022-04-10 +0900
categories: [CS, js]
tags: [functional]
---

# 함수와 타입

## 함수 합성

함수의 합성(composition) 이란한 함수의 공역이, 다른 함수의 정의역과 일치하는 경우 두 함수를 이어 하나의 함수로 만드는 연산이다

수학에서 함수는 어떤 집합의 각 원소를 다른 집합의 유일한 원소에 대응시키는 이항관계다.
입력값들의 집합을 정의역이라 한다
이 함수 값들이 속하는 더 큰 집합을 공역이라 한다

수학에서 집합은 특정한 조건에 맞는 별개의 원소들의 모임

- (total) function
- 정의역의모든원소에대해함수가정의되어야한다

- partial function 부분함수
- 가능한입력중에일부에만반환값이정의되어있다

```javascript
export function priceOfTomato() {
  return 7000;
}

export function priceOfOrange() {
  return 15000;
}

export function priceOfApple() {
  return 10000;
}

export function getPrice(name: string) {
  if (name === "tomato") {
    return 7000;
  } else if (name === "orange") {
    return 15000;
  } else if (name === "apple") {
    return 10000;
  }
}
// typescript의 추론
function getPrice(name: string): 7000 | 10000 | 15000 | undefined
```

### 함수 합성해보기

```javascript
/*
토마토: 7000원
오렌지: 15000원
사과: 10000원
*/

export function getPrice(name: string): number | undefined {
  if (name === "tomato") {
    return 7000;
  } else if (name === "orange") {
    return 15000;
  } else if (name === "apple") {
    return 10000;
  }
}

export const isExpensive = (price: number | undefined) => {
  // getPrice의 결과에 undefined가 포함 되어 있기 때문에
  // getPrice와 합성을 위해 undefined에 대한 처리 필요
  if (price === undefined) {
    return false;
  }
  return price > 10000;
};

// isExpensive와 getPrice를 합성해서 하나의 함수로
export const isExpensivePrice = (name: string): boolean => {
  return isExpensive(getPrice(name));
};

export const main = () => {
  return isExpensive(getPrice("tomato"));
};
```

## 제네릭

```typescript
// 숫자를 그대로 돌려주는 함수
export const idNumber = (n: number) => {
  return n;
};

// 문자열을 그대로 돌려주는 함수
export const idString = (s: string) => {
  return s;
};

// boolean값을 그대로 돌려주는 함수
export const idBoolean = (b: boolean) => {
  return b;
};

// 제네릭 타입을 매개변수화 시킴 <>
// 어떤 타입의 값이라도 그대로 돌려주는 함수
export const id = <T>(x: T) => {
  return x;
};
// 임의의 T 타입을 받아 T타입을 반환하는 함수

// 주석을 제거하면 에러 발생
export type T1 = Array;

export type T2 = Array<string>;

// export const compose = (isExpensive, getPrice) => (name) => {}

// export const compose = (g, f) => (x) => {}

// export const compose = (g, f: (s: string) => number | undefined) => (x: string) => {}

// export const compose = (g: (y: number | undefined) => boolean, f: (s: string) => number | undefined) => (x: string) => {}

/* export const compose = (g: (y: number | undefined) => boolean, f: (s: string) => number | undefined) => (x: string) => {
  retrun g(f(x))
}
*/

// s와 x가 string으로 같으므로 A로 매개변수화
// number | undefined 를 B로 매개변수화
// boolean을 C로 매개 변수화

// 매개 변수화로 인해서 타입은 바뀌어도
// 같아야 할 부분에 있어서는 같게 설정이 가능
export const compose =
  <A, B, C>(g: (y: B) => C, f: (x: A) => B) =>
  (x: A) => {
    return g(f(x));
  };

// compose 함수의 원래 타입
// <A, B, C>(g: (y: B) => C, f: (x: A) => B) => (x: A) => C

// 매개변수를 지워서 단순하게 표기
// <A, B, C>((B) => C, (A) => B) => (A) => C
```

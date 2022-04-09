---
title: 함수형 프로그래밍 - 함수와 타입 - 1
author: YHole
date: 2022-04-09 +0900
categories: [CS, js]
tags: [functional]
---

# 함수와 타입

## 방향 잡기

- 전역변수에 숨겨진 부수효과 찾아내기
- 절차(Procedure)를 순수함 수로 바꾸기
- RecordData와 함수 비교하기
- 함수 합성(composition)
- 함수와 타입, 집합
- 타입을 만드는 함수, 제네릭(Generic)

### 실습 코드

```javascript
/*
토마토: 7000원
오렌지: 15000원
사과: 10000원
*/

export let totalPrice = 0;
totalPrice += 7000;
totalPrice += 15000;
totalPrice += 10000;
// 오렌지가 2개인지, 사과가 3개인지 불분명
totalPrice += 30000;
// 사소한 오타로 인한 실수
totalPrice += 8000;
// 단위가 잘못됐다면 실수라는 것을 인지하기조차 어려움
totalPrice += 70000;
```

- 명령형

```javascript
// 전역 변수라는 부수효과를 사용
// 예측하기 어려운 오류의 발생 가능성이 있음
export let totalPrice = 0;

export function addTomato() {
  // 부수효과
  totalPrice += 7000;
}

export function addOrange() {
  totalPrice += 15000;
}

export function addApple() {
  totalPrice += 10000;
}

export function list1() {
  // 토마토, 오렌지, 사과 한 상자
  addTomato();
  addOrange();
  addApple();
}

export function list2() {
  // 토마토 2상자
  addTomato();
  addTomato();
}

export function list3() {
  // addOrange 함수를 직접 100번 호출 하는 대신
  // 함수를 100번 호출 하도록 명령하는 코드를 작성
  // 오렌지 100상자
  for (let i = 0; i < 100; i++) {
    addOrange();
  }
}

export function main() {
  list3();
}
```

### 부수 효과 없이 작성

- 전역 변수가 없기 때문에 호출 횟수에 영향 없음

```javascript
/*
토마토: 7000원
오렌지: 15000원
사과: 10000원
*/
export function priceOfTomato() {
  return 7000;
}

export function priceOfOrange() {
  return 15000;
}

export function priceOfApple() {
  return 10000;
}

export function list1() {
  // 토마토, 오렌지, 사과 한 상자
  return priceOfTomato() + priceOfOrange() + priceOfApple();
}

export function list2() {
  // 토마토 2상자
  return priceOfTomato() + priceOfTomato();
}

export function list3() {
  // 오렌지 100상자 구입은 오렌지 가격을 100번 더하는 것과 동일하기 때문에
  // 오렌지 가격 * 100 으로 계산 가능
  // 오렌지 100상자
  return priceOfOrange() * 100;
}

export function getTotalPrice() {
  return list2();
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
// 함수를 데이터로 표현
export const priceOfFruit = {
  tomato: 7000,
  orange: 15000,
  apple: 10000,
};

// 대량의 데이터를 직접 입력하는 것은 어려움
export const isEven = {
  tomato: true, // 문자열이 짝수인지지
  orange: true,
  apple: false,
};

// 어떤 문자열이여도 가능(함수로 한번에)
export const isEvenFn = (str: string) => str.length % 2 === 0;
```

---
title: 함수형 프로그래밍 - Option 없을 수도, 실패할 수도 있는 값
author: YHole
date: 2022-04-13 +0900
categories: [Front end, js]
tags: [functional]
---

# 입력에 대응되는 출력이 없을 때

- 주어진인자에대응되는반환값이없으면어떻게해왔나?

> 타입에포함된임의의값을대신반환하기
> 예시) -1, 0, null, undefined, Infinity, NaN, …

```typescript
Array의 map
(arr: Array<A>, f: (a: A) => B) => Array<B>
Option의 map
(oa: Option<A>, f: (a: A) => B) => Option<B>
```

```javascript
const arr = ["apple", "banana", "strawberry"];
arr.indexOf("banana"); // 배열에서위치인덱스찾기
// 1
arr.indexOf("Onion"); // 배열에찾는값이없을때?
// -1

const div = (a, b) => a / b;
div(10, 5); // 나눌수있는경우
// 2
div(10, 0); // 0으로나눌수있나?
// Infinity
div(0, 0); // 0을0으로?
// NaN
```

- 할인 금액 항목이 있거나 없거나 한 경우

```typescript
const stockItem = (item: Item): string => {
  let saleText = "";
  let discountPrice = 0;
  if (item.discountPrice !== undefined) {
    saleText = `(${item.discountPrice}원 할인)`;
    discountPrice = item.discountPrice;
  }

  return `
    <li>
      <h2>${item.name}</h2>
      <div>가격: ${item.price - discountPrice}원 ${saleText}</div>
      <div>수량: ${item.quantity}상자</div>
    </li>
  `;
};

const outOfStockItem = (item: Item): string => `
  <li class="gray">
    <h2>${item.name} (품절)</h2>
    <div class='strike'>가격: ${item.price}원</div>
    <div class='strike'>수량: ${item.quantity}상자</div>
  </li>
`;

const item = (item: Item): string => {
  if (item.outOfStock) {
    return outOfStockItem(item);
  } else {
    return stockItem(item);
  }
};

const totalCalculator = (
  list: Array<Item>,
  getValue: (item: Item) => number
) => {
  let result: Array<number> = [];
  list.forEach(function (item) {
    if (item.outOfStock === false) {
      result.push(getValue(item));
    }
  });

  return result.reduce((total, value) => total + value, 0);
};

const totalCount = (list: Array<Item>): string => {
  const totalCount = totalCalculator(list, (item) => item.quantity);
  return `<h2>전체 수량: ${totalCount}상자`;
};

const totalPrice = (list: Array<Item>): string => {
  const totalPrice = totalCalculator(
    list,
    (item) => item.price * item.quantity
  );

  const totalDiscountPrice = totalCalculator(list, (item) => {
    let discountPrice = 0;
    if (item.discountPrice !== undefined) {
      discountPrice = item.discountPrice;
    }
    return discountPrice * item.quantity;
  });

  return `<h2>전체 가격: ${
    totalPrice - totalDiscountPrice
  }원 (총 ${totalDiscountPrice}원 할인)</h2>`;
};

const list = (list: Array<Item>) => {
  return `
    <ul>
    ${list
      // 1. 목록에 있는 모든 아이템을 태그로 변경
      .map(item)
      // 2. 태그의 목록을 모두 하나의 문자열로 연결
      .reduce((tags, tag) => tags + tag, "")}
    </ul>
  `;
};

const app = document.getElementById("app");
if (app != null) {
  app.innerHTML = `
  <h1>장바구니</h1>
  ${list(cart)}
  ${totalCount(cart)}
  ${totalPrice(cart)}
  `;
}
```

## Option을 사용하기

```typescript
// 값이 있을수도, 없을수도 있는 자료구조.

export type Some<A> = {
  readonly _tag: "some";
  readonly value: A;
};

export type None = {
  readonly _tag: "none";
};

export type Option<A> = Some<A> | None;

// 타입을 구현해주는 함수 만들기
export const some = <A>(value: A): Option<A> => ({ _tag: "some", value });

export const none = (): Option<never> => ({ _tag: "none" });

export const isSome = <A>(oa: Option<A>): oa is Some<A> => oa._tag === "some";

export const isNone = <A>(oa: Option<A>): oa is None => oa._tag === "none";

export const fromUndefined = <A>(a: A | undefined): Option<A> => {
  if (a === undefined) return none();
  return some(a);
};

export const getOrElse = <A>(oa: Option<A>, defaultValue: A): A => {
  // 값이 없으면 지정된 값을 사용하고
  if (isNone(oa)) return defaultValue;
  // 값이 있다면 해당 값을 사용한다.
  return oa.value;
};

export const map = <A, B>(oa: Option<A>, f: (a: A) => B): Option<B> => {
  // 값이 없으면 값이 없는 상태를 유지한다.
  if (isNone(oa)) return oa;
  // 값이 있으면 값을 함수에 적용한다.
  return some(f(oa.value));
};

export const mapOrElse = <A, B>(
  oa: Option<A>,
  f: (a: A) => B,
  defaultValue: B
): B => {
  return getOrElse(map(oa, f), defaultValue);
};
```

- 리팩토링

```typescript
import "./index.css";
import { cart, Item } from "./cart";
import * as O from "./option";

const stockItem = (item: Item): string => {
  const optionDiscountPrice = O.fromUndefined(item.discountPrice);
  // let discountPrice = 0;
  const discountPrice = O.getOrElse(optionDiscountPrice, 0);

  const saleText = O.mapOrElse(
    optionDiscountPrice,
    (discountPrice) => `${discountPrice}원 할인`,
    ""
  );

  /*
  const optionSaleText = O.map(
    optionDiscountPrice,
    (discountPrice) => `${discountPrice}원 할인`
  );
  const saleText = O.getOrElse(optionSaleText, "");
  */

  // let saleText = "";
  // if (O.isSome(optionDiscountPrice)) {
  //   saleText = `(${discountPrice}원 할인)`;
  // }

  return `
    <li>
      <h2>${item.name}</h2>
      <div>가격: ${item.price - discountPrice}원 ${saleText}</div>
      <div>수량: ${item.quantity}상자</div>
    </li>
  `;
};

const outOfStockItem = (item: Item): string => `
  <li class="gray">
    <h2>${item.name} (품절)</h2>
    <div class='strike'>가격: ${item.price}원</div>
    <div class='strike'>수량: ${item.quantity}상자</div>
  </li>
`;

const item = (item: Item): string => {
  if (item.outOfStock) {
    return outOfStockItem(item);
  } else {
    return stockItem(item);
  }
};

const totalCalculator = (
  list: Array<Item>,
  getValue: (item: Item) => number
) => {
  let result: Array<number> = [];
  list.forEach(function (item) {
    if (item.outOfStock === false) {
      result.push(getValue(item));
    }
  });

  return result.reduce((total, value) => total + value, 0);
};

const totalCount = (list: Array<Item>): string => {
  const totalCount = totalCalculator(list, (item) => item.quantity);
  return `<h2>전체 수량: ${totalCount}상자`;
};

const totalPrice = (list: Array<Item>): string => {
  const totalPrice = totalCalculator(
    list,
    (item) => item.price * item.quantity
  );

  const discountPrice = totalCalculator(list, (item) => {
    // 파이프라인 오퍼레이터
    // item.discountPrice |> O.fromUndefined(%) |> O.getOrElse(%, 0)
    const discountPrice = O.getOrElse(O.fromUndefined(item.discountPrice), 0);
    return discountPrice * item.quantity;
  });

  return `<h2>전체 가격: ${
    totalPrice - discountPrice
  }원 (총 ${discountPrice}원 할인)</h2>`;
};

const list = (list: Array<Item>) => {
  return `
    <ul>
    ${list
      // 1. 목록에 있는 모든 아이템을 태그로 변경
      .map(item)
      // 2. 태그의 목록을 모두 하나의 문자열로 연결
      .reduce((tags, tag) => tags + tag, "")}
    </ul>
  `;
};

const app = document.getElementById("app");
if (app != null) {
  app.innerHTML = `
  <h1>장바구니</h1>
  ${list(cart)}
  ${totalCount(cart)}
  ${totalPrice(cart)}
  `;
}
```

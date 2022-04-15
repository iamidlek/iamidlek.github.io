---
title: 함수형 프로그래밍 - map
author: YHole
date: 2022-04-15 +0900
categories: [Front end, js]
tags: [functional]
---

# map

```typescript
import * as O from "./option";

export const curry2 =
  <A, B, C>(f: (a: A, b: B) => C) =>
  (a: A) =>
  (b: B): C =>
    f(a, b);

export const flip =
  <A, B, C>(f: (a: A, b: B) => C) =>
  (b: B, a: A): C =>
    f(a, b);

// Array<A> == A[]
// map :: (Array<A>, (A => B)) => Array<B>
export const map = <A, B>(array: Array<A>, f: (a: A) => B): Array<B> => {
  const result: Array<B> = [];
  for (const value of array) {
    result.push(f(value));
  }
  return result;
};

export const main = () => {
  const numbers = [1, 2, 3];
  const isEven = (x: number) => x % 2 === 0;

  map(numbers, isEven);

  // curriedMap :: Array<A> => ((A => B) => Array<B>)
  const curriedMap = curry2(map);
  curriedMap(numbers)(isEven);

  // map :: Array<A> ~> (A => B) => Array<B>
  // 인스턴스로서 메서드를 호출
  numbers.map(isEven);

  // map_ :: (A => B) => (Array<A> => Array<B>)
  const map_ = curry2(flip(map));
  map_(isEven)(numbers);

  // isEven :: number => boolean
  // mapIsEven :: Array<number> => Array<boolean>
  const mapIsEven = map_(isEven);

  isEven(42);
  isEven(7);

  mapIsEven(numbers);
  mapIsEven([]);
  mapIsEven([42]);

  const omap = curry2(flip(O.map));
  // optionIsEven :: Option<number> => Option<boolean>
  const optionIsEven = omap(isEven);
  optionIsEven(O.some(42));
  optionIsEven(O.none());
};
```

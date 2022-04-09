---
title: 함수형 프로그래밍 - 온보딩
author: YHole
date: 2022-04-08 +0900
categories: [CS, js]
tags: [functional]
---

# 객체지향과 함수형의 차이

- 명사의 왕국인 객체지향 세계, 동사의 왕국인 함수형 세계 - STEVEYEGGE
- 100개의 함수를 하나의 자료구조에 적용하는 것이
  10개의 함수를 10개의 자료구조를 적용하는 것보다 낫다.  
  -엘런펄리스(튜링상을수상한최조의컴퓨터과학자)

- 객체지향프로그래밍은 움직이는 부분을 캡슐화하여 코드 이해를 돕고,
  함수형 프로그래밍은 움직이는 부분을 최소화하여 코드 이해를 돕는다.  
  -마이클페더스(Object 멘토,레거시코드활용전략의저자)

- OOP와 함수적 계산은 완전히 조화될수있습니다. (그리고그래야만하죠!)  
  -AlanKay(객체지향과SmallTalk언어창시자)

## Side Effect (부수 효과)

> 값을 반환하는것 외에 부수적으로 일으키는 효과
> "변수나 상태를 바꾸거나 수정"
> "화면이나 파일에 데이터를 쓰는 IO 작업"
> "다른 부수효과가 있는 함수나 상태 값에 의존"

## 순수 함수 pure function

- 똑같은 매개변수(입력)를 받으면 항상 같은 값을 반환하는 함수
  > 부수효과 없음
  > 수학의 함수-> 증명가능, 안전
  > 순서나 실행횟수랑 상관없이, 항상 예측 가능한 결과

```javascript
// 상태가 변한다
// 복잡하다 합성하기 어렵다
function sum_1_to_100() {
  let sum = 0;
  for (let i = 1; i <= 100; i++) {
    sum += i;
  }
  return sum;
}
console.log(sum_1_to_100());
```

```javascript
// 순수하지만 장황한 재귀
function sum_1_to_100(){​
  function go(sum,i) {
    if(i> 100){​
      return sum;​
    }​
    return go(sum +i, i+1);​
  }
  return go(0, 1);​
}
console.log(sum_1_to_100())
```

```javascript
function loop(fn, acc, list) {
  if (list.length === 0) return acc;
  const [head, ...tail] = list;
  return loop(fn, fn(acc, head), tail);
}

const range = (start, end) =>
  Array.from({ length: end - start + 1 }, (_, index) => index + start);

const plus = (a, b) => a + b;

console.log(loop(plus, 0, range(1, 100)));
```

## 효과를 안전하게 추상화하기

- 숨겨진 부수효과? => 명확하고 순수한 Effect
- 뒤섞인 코드? 효과와 계산이 직교하게 분리
- 보일러 플레이트? => 재사용하기 쉬운 추상화
- 제각각인 사용법? 단순하고 일관된 인터페이스

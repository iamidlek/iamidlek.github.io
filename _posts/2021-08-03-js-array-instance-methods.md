---
title: js-array-instance-methods
author: YHole
date: 2021-08-03 +0900
categories: [Front end, js]
tags: [js, array, 배열, instance methods, 내장 함수, 고차 함수]
---

## 일급 객체(First-class citizen) ?

자바스크립트에서 `함수`는 `일급 객체`이다

일급 객체의 세가지 조건은 이하와 같다

- 변수에 할당할 수 있다  
  
```javascript
const sample = function(something){
  // 내용
  return
}
console.log(sample(something))
```

- 다른 함수의 인자로 전달될 수 있다

```javascript
// callback 함수
function callback (check) {
  return check === 'd' ? 'r' : 'l'
}
// caller 함수
function caller (fn, char) {
  const text = char
  return fn(text)
}

caller(callback,'d')
// r
```

- 다른 함수의 결과로서 리턴될 수 있다

```javascript
// union은 다른 함수를 return하므로 고차함수
function union(first) {
  // 익명의 함수가 retrun됨
  return function (second) {
    return first + second;
  };
}

const text = union('성');
console.log(text('명'));
// 성명

console.log(union(11)(17))
// 28
```

## 배열 내장 고차 함수 (Higher order function)

### forEach()

- for문 대신 사용 가능
- 모든 배열 요소가 순회하는 메소드
- `아무것도 반환하지 않는다`
- 원본 배열에 변화 없음

```javascript
const narray = [1, 2, 3];
const result = [];

narray.forEach(function(n) {
  result.push(n+5);
});

console.log(result)
// [ 6, 7, 8 ]
```


### map()

- 모든 배열 요소가 순회하는 메소드
- 함수를 통과한 결과가 새로운 `배열`로 return 된다
- return {} 객체로 반환하면 객체가 들어있는 `배열`이 된다
- 원본 배열에 변화 없음

```javascript
const array = [1, 2, 3];

// 함수를 입력
const map = array.map(x => x * x);

// 함수를 미리 선언
const fx = function (x) {
  return x * x
}
const fn = x => x / x

console.log(map);
// [ 1, 4, 9 ]
console.log(map.map(fx))
// [ 1, 16, 81 ]
console.log(map.map(fn))
// [ 1, 1, 1 ]
console.log(array)
// [ 1, 2, 3 ]
```

### filter()

- 모든 배열 요소가 순회하는 메소드
- `조건을 통과한` 결과가 새로운 배열로 return 된다
- 원본 배열에 변화 없음

```javascript
const array = [1, 2, 3, 4];
// 조건이 true 면 해당 값이 bigger로 들어옴
const bigger = array.filter( x => x > 2);

console.log(bigger)
// [ 3, 4 ]
```

### reduce()

- 모든 배열 요소가 순회하는 메소드
- 작동 순서  
  1. accumulator에 초기값이 들어간다
  2. current에 배열[0] 값이 들어간다
  3. 함수의 결과가 accumulator에 들어간다  

  - 초기값이 없는 경우
    - accumulator에 배열[0]
    - currnet에 배열[1]
- 순회를 완료했을때 나오는 `accumulator값을 반환`한다
- 원본 배열에 변화 없음

```javascript
const array = [1, 2, 3, 4];

const sum = array.reduce((accumulator, current) => {
  return accumulator + current;
}, 0);

console.log(sum)
// 10

// 구매 가능한 목록중 가장 큰 price 를 가진 상품을 반환
const max = purchasable.reduce(function (previous, current) {
  return previous.price > current.price ? previous : current;
});

// 객체의 price의 값을 비교
// 조건에 따라 previous 혹은 current가 반환됨
```

### find()

- 해당하는 첫 번째 값을 반환하는 메소드
- 배열에 해당하는 값이 없다면 undefined를 반환
- 원본 배열에 변화 없음

```javascript
const array = [1, 2, 3, 4, 5];
const result = array.find(x => x > 3);
console.log(result); 
// 4
```

### sort()

- 원본 배열에 변화 있음
- 배열 내부 값을 문자열로 인식한다
- 원하는 sorting을 위해선 compare function정의가 필요

```javascript
// array.sort([compareFunction])

const unsorted = [3, 1, 5, 4, 2];

// a,b에 순차적인 값이 들어오지는 않는다
unsorted.sort(function(a, b) {
  // 결과가 음수면 자리를 바꾸지 않는다
  // 결과가 양수면 자리를 바꾼다
  return a - b;
  // 오름차순
});

console.log(unsorted); 
// [1, 2, 3, 4, 5]
```

### some()

- 조건을 `하나 이상 통과`하면 true를 반환
- 통과한 것이 없다면 false를 반환
- 빈 배열은 false를 반환
- 원본 배열에 변화 없음

```javascript
// 조건
const comp = x => x >= 5;

const arrayt = [1, 2, 3, 4, 5];
console.log(arrayt.some(comp)); 
// true

const arrayf = [1, 2, 3, 4];
console.log(arrayf.some(comp)); 
// false
```

### every()

- 조건을 `모두 통과`해야 true를 반환
- 빈 배열은 true를 반환
- 원본 배열에 변화 없음

```javascript
// 조건
const comp = x => x < 5;

const arrayt = [1, 2, 3, 4, 5];
console.log(arrayt.every(comp)); 
// false

const arrayf = [1, 2, 3, 4];
console.log(arrayf.every(comp)); 
// true
```

## 배열 내장 함수

- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array#%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4_%EB%A9%94%EC%84%9C%EB%93%9C)

### splice

- 원본 배열에 변화 있음
- 특정 항목 제거 또는 추가
- splice(index, 몇개 삭제)

```javascript
// 제거
const overlap = ['a', 'b', 'c', 'c', 'c','d'];
const adjust = overlap.splice(3, 2);
console.log(overlap) // [ 'a', 'b', 'c', 'd' ]
console.log(adjust) // [ 'c', 'c' ]

// replace
const replace = ['a', 'b', 'f', 'g'];
const removed = replace.splice(0, 2, 'c', 'd', 'e');
console.log(replace) // [ 'c', 'd', 'e', 'f', 'g' ]
console.log(removed) // [ 'a', 'b' ]

// add
const alpha = ['a', 'b', 'e', 'f'];
// 삭제할 개수 0 이면 추가
const add = alpha.splice(2, 0, 'c', 'd');
console.log(alpha) // [ 'a', 'b', 'c', 'd', 'e', 'f' ]
console.log(add) // []
```

### slice

- 원본 배열에 변화 없음
- 지정 부분을 복사하여 배열 반환

```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// ["camel", "duck"]

console.log(animals.slice(-2));
// ["duck", "elephant"]

console.log(animals.slice(2, -1));
// ["camel", "duck"]

console.log(animals.slice(-4, -1));
// [ 'bison', 'camel', 'duck' ]
```

### shift & unshift

- 배열의 맨 앞부분에 삭제와 추가
- shift의 경우 삭제된 값을 반환
- 빈 배열일 경우 undefined를 반환
- unshift의 추가된 후의 배열 길이를 반환
- 원본 배열에 변화 있음 

```javascript
const array = [1, 2, 3];

const firstElement = array.shift();

console.log(array);
// [2, 3]
console.log(firstElement);
// 1

console.log(array.unshift(4, 5));
// 4
console.log(array);
// [4, 5, 2, 3]
```

### pop & push

- 배열의 맨 끝부분에 삭제와 추가
- pop의 경우 삭제된 값을 반환
- 빈 배열일 경우 undefined를 반환
- push의 경우 추가된 후의 배열 길이를 반환
- 원본 배열에 변화 있음 

```javascript
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato'];

console.log(plants.pop());
// "tomato"
console.log(plants);
// ["broccoli", "cauliflower", "cabbage", "kale"]

console.log(plants.push('cucumber'));
// 5
console.log(plants);
// [ 'broccoli', 'cauliflower', 'cabbage', 'kale', 'cucumber' ]
```

### concat

- 합쳐진 배열을 반환
- 원본 배열에 변화 있음

```javascript
const alpha = ['a', 'b', 'c'];
const numeric = [1, 2, 3];

alpha.concat(numeric);
// ['a', 'b', 'c', 1, 2, 3]

// 3개를 합치려면
const num1 = [1, 2, 3];
const num2 = [4, 5, 6];
const num3 = [7, 8, 9];

console.log(num1.concat(num2, num3))
// [1, 2, 3, 4, 5, 6, 7, 8, 9]

// 하나의 배열에 값이 추가됨
const test = ['a', 'b', 'c'];
console.log(test.concat(1, [2, 3]))
// ['a', 'b', 'c', 1, 2, 3]
```

### join

- 문자열을 반환
- 원본 배열에 변화 없음

```javascript
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// Fire,Air,Water

console.log(elements.join(''));
// FireAirWater

console.log(elements.join('-'));
// Fire-Air-Water
```

### indexOf

- 해당하는 해당하는 첫번째 요소의 인덱스 번호를 반환하는 메소드
- 배열에 해당하는 값이 없다면 -1을 반환
- 원본 배열에 변화 없음

```javascript
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];

console.log(beasts.indexOf('bison'));
// 1

// 탐색을 시작하는 인덱스 지정 : 2 부터
console.log(beasts.indexOf('bison', 2));
// 4

console.log(beasts.indexOf('giraffe'));
// -1
```

### findindex

- 조건에 해당하는 첫번째 요소의 인덱스 번호를 반환하는 메소드
- 배열에 해당하는 값이 없다면 -1을 반환
- 원본 배열에 변화 없음

```javascript
const array = [1, 2, 10, 20, 100, 200];

const fnAlsoOk = x => x > 20;
console.log(array.findIndex(fnAlsoOk));
// 4
```

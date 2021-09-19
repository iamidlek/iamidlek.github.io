---
title: js-copy-array
author: YHole
date: 2021-09-08 +0900
categories: [Front end, js]
tags: [js, copy, shallow, deep, array]
---

## 복사

> 단순복사는 완전히 동일한 주소  

> 얕은복사(shallow copy)는 껍데기만 다른 주소  
  내용은 동일한 주소  

> 깊은복사(deep copy)는 재귀적 복사로 전부 다른 주소

### 배열

```js
let age = 20;
let age2 = age;
console.log(age, age2);
// 20 20
age = 200;
console.log(age, age2);
// 200 20
```

```js
const arr = [ 1, 'yoo', ['kim', 3]];
const temp = arr;

temp[3] = 'ro';

console.log(arr,temp)
// 같은 값을 반환 (원본 배열이 변한다)
```

- 얕은 복사가 일어나는 경우
  - 내부에 참조 객체가 없다면 깊은 복사

```js
const temp2 = arr.slice();
const temp3 = [].concat(arr);
const temp4 = [...arr];
const temp5 = Array.from(arr);
```

> 가장 밖의 배열에 있는 요소는 immutable  
> 그 안에 배열이나 객체가 있으면 mutable로  
> 배열 안의 배열, 객체의 내용을 변경하면 원본도 변경된다

### 깊은 복사

- 위의 설명처럼 배열 안에 참조형 요소가 있다면 얕은 복사가 된다

> 깊은 복사를 위해선 재귀적인 복사를 하는 반복문을 사용  
> 즉 사용자가 직접 정의해야 한다

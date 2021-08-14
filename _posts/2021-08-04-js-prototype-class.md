---
title: js-prototype-class
author: YHole
date: 2021-08-04 +0900
categories: [Front end, js]
tags: [js, object, 프로토타입, prototype, class, 클래스]
---

## prototype 활용

어떠한 함수를 반복적으로 사용한다고 생각해보면
매번 같은 부분은 메모리의 낭비로 이어진다
공통적으로 사용가능한 부분을 관리하는 방법을 확인해 보자


```javascript
// 생성자 함수는 파스칼케이스
function Feat(a,b){
  this.uni = a
  this.que = b    
}
Feat.prototype.another = function () {
  return `${this.uni} is ${this.que}`
}

const z = new Feat('apple','fruits')
const y = new Feat('samsung','corporation')
const x = new Feat('kim','student')

console.log(z)
// Feat { uni: 'apple', que: 'fruits' }
console.log(y)
// Feat { uni: 'samsung', que: 'corporation' }

console.log(z.another())
// apple is fruits
console.log(y.another())
// samsung is corporation
console.log(x.another())
// kim is student
```

- 공통적으로 사용해도 좋은 부분은 .prototype.으로 만든다

## Class 활용

```javascript
class Feat{
  // 필수로 작성해야 하는 부분
  constructor(a, b) {
    this.uni = a
    this.que = b
  }    
  // prototype 축약형으로 작성
  another () {
    return `${this.uni} is ${this.que}`
  }
}

// 같은 결과가 나온다
```

### Class 상속

어떻게 상속을 하는지 확인해 보자

```javascript
class Student {
  constructor(num, name) {
    this.num = num
    this.name = name
  }
}

class Grade extends Student {
  constructor(num, name, grade){
    // num과 name이 super를 통해 부모 클래스에 전달
    super(num,name)
    this.grade = grade
  }
}

class Gender extends Student {
  constructor(num, name, gender) {
    super(num, name)
    this.gender = gender
  }
  studentInfo () {
    return `${this.num} - ${this.name} : ${this.gender}`
  }
}

const a = new Gender('1', 'kim', 'female')
const b = new Gender('2', 'yoo', 'male')

console.log(a)
// Gender { num: '1', name: 'kim', gender: 'female' }
console.log(b)
// Gender { num: '2', name: 'yoo', gender: 'male' }

console.log(a.studentInfo())
// 1 - kim : female
console.log(b.studentInfo())
// 2 - yoo : male
```

## 헷갈림 포인트

- 멤버 : 함수의 속성과 메서드를 의미
- 속성 : property 키와 값으로 이루어짐
- 메서드 : 키와 함수로 이루어짐(키=메소 지명으로 생략 가능)
- 화살표 함수  
  - const name () => { } 방식으로 선언한다

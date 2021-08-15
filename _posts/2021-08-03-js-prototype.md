---
title: js-prototype
author: YHole
date: 2021-08-03 +0900
categories: [Front end, js]
tags: [js, object, prototype]
---

## prototype 정의

- MDN 문서를 읽어도 잘 모르겠다

> 자바스크립트는 동적인 언어이며 클래스가 없다.  
> ES2015부터 class 키워드를 지원하기 시작했으나,  
> 문법적인 양념일 뿐이며 자바스크립트는 여전히 프로토타입 기반의 언어다

> 상속 관점에서 자바스크립트의 유일한 생성자는 객체뿐이다.  
> 각각의 객체는 Prototype이라는 은닉(private) 속성을 가지는데  
> 자신의 프로토타입이 되는 다른 객체를 가리킨다.
> 그 객체의 프로토타입 또한 프로토타입을 가지고 있고 이것이 반복되다  
> 결국 null을 프로토타입으로 가지는 오브젝트에서 끝난다.  
> null은 더 이상의 프로토타입이 없다고 정의되며  
> 프로토타입 체인의 종점 역할을 한다.


## Unfamiliar

### prototype(원형, 원본, 원조) ?

> 객체의 원형인(원조) 프로토타입을 이용하여 새로운 객체를 만들어내는 기법  
> 이렇게 만들어진 객체 역시 자기자신의 프로토타입을 갖는다  
> 이 새로운 객체의 원형을 이용하면 또 다른 새로운 객체를 만들수 있으며  
> 이런 구조로 객체를 확장하는 방식이 프로토타입 기반 프로그래밍이다

- instance의 정보를 보면 프로토타입에 sample 함수가 보인다
- 여기서의 프로토타입 sample() {} 원형 그 자체일까? → No

```javascript
function sample() {}
const instance = new sample();
console.log(instance)
// [[Prototype]]: Object
// constructor: ƒ sample()
// arguments: null
// caller: null
// length: 0
// name: "sample"
// prototype: {constructor: ƒ}
```
### prototype property(자산, 특질) ?

> 모든 함수 객체의 Constructor는 prototype이란 property를 가지고 있다  
> 이 prototype property는 객체가 생성될 당시 만들어지는 객체 자신의 원형을 가리킨다  
> prototype property는 자신을 만든 원형이 아닌 자신을 통해  
> 만들어질 객체들이 원형으로 사용할 객체를 말한다

- 자바스크립트의 모든 객체는 생성과 동시에 자기자신이 생성될 당시의 정보를 취한 
- Prototype Object 라는 새로운 객체를 Cloning 하여 만들어낸다. 
- 프로토타입이 객체를 만들어내기위한 원형이라면, `Prototype Object`는 자신의 분신이며
- 자신을 원형으로 만들어질 `다른 객체가 참조`할 프로토타입이 된다.

- 위 예의 prototype에 대한 link는 `Prototype Object`의 정보이며
- prototype property는 자신을 원형으로 만들어질 
- 새로운 객체들, 하위로 물려줄 연결에 대한 속성이 된다

- 확인해 보기  

```javascript
function sample(x) {
  this.something = x;
};

const instance = new sample('abc');
console.log(instance)
// sample { something: 'abc' }
console.log(instance.something)
// abc

console.log(sample.prototype.something)
// undefined
console.log(instance.prototype.something)
// TypeError: Cannot read property 'something' of undefined
```

- prototype(이라는) property는 함수 객체만 가지고 있다
- 즉 Constructor(생성자)가 가지는 property이다

- instance는 `Prototype Object`에서 확장된 단일 객체이다
- 즉 prototype(이라는) property를 소유하지 않는다


![relation](/assets/img/docsitem/struct.png)

[참조](https://www.nextree.co.kr/p7323/)

### relation detail

> 이해에 도움이 되는 [포스트](http://insanehong.kr/post/javascript-prototype/)를 인용

```javascript
//#예제 1.
var A = function () {
    this.x = function () {
         console.log('hello');
    };
};
A.x=function() {
    console.log('world');
};
var B = new A();
var C = new A();
B.x();
> hello
C.x();
> hello

//#예제 2.
var A = function () { };
A.x=function() {
    console.log('hello');
};
A.prototype.x = function () {
     console.log('world');
};
var B = new A();
var C = new A();
B.x();
> world
C.x();
> world
```
> B,C 를 생성하기 위한 객체 원형 프로토타입은 A 이다. 하지만 여기서 반드시 집고 넘어가야하는 사실은 B,C는 A 를 프로토타입으로 사용하기위해서 A의 `prototype Object`를 사용한다는 것이다. 그리고 이 Prototype Object는 A 가 생성될 당시의 정보만을 가지기 때문에 예제1에서 A의 Prototype Object가 알고 있는 x 는 'hello'가 된다.  
즉 A.x 를 아무리 수정하여도 A의 `Prototype Object`는 변경되지 않기 때문에 A 를 프로토타입으로 생성되는 B,C는 'hello'만 참조하는 것이다.

> 예제2 에서의 결과가 world 가 되는 이유도 같은 이유다.  
A.prototype은 A의 `Prototype Object`를 지목하기 때문에  
A.prototype.x를 정의한다는 것은 A의 `Prototype Object`를  
정의하는 것이고 만들어지는 B,C는 'world'가 되는 것이다.

### Prototype Chain

```javascript
var A = function () { };
A.x=function() {
    console.log('hello');
};

A.x(); // hello

A.prototype.x = function () {
     console.log('world');
};

var B = new A();

B.x(); // world

A.prototype.x = function () {
     console.log('boom');
};

A.x(); // hello
B.x(); // boom
```

- Prototype Chain

> 객체의 생성 과정에서 모태가 되는 프로토타입과의 연결고리가 이어져  
> 상속관계를 통하여 상위 프로토타입으로 연속해서 이어지는 관계를  
> 프로토타입 체인이라고 한다. `__proto__` 를 따라 올라가게 된다.

> 하위 객체에서 상위객체의 프로퍼티와 메소드를 상속받는다.  
> 그리고 동일한 이름의 프로퍼티와 메소드를 재정의 하지 않는 이상  
> 상위에서 정의한 내용을 그대로 물려받는다.
 
> B는 A prototype Object를 프로토타입으로 만들어졌음에도 불구하고  
> x 라는 프로퍼티가 존재하지 않는다.  
> 즉 하위 객체는 상위 객체의 속성과 메소드를 상속 받는 것이 아니라  
> **공유**하고 있는 것이다

![relation](/assets/img/docsitem/relation3.png)

- 공유 

```javascript
var A= function() {};
var B = new A();
A.prototype.x='hello';
console.log(B);
```

- 상속

```javascript
var A= function() {this.x='hello';};
var B = new A();
console.log(B);
```

[관련자료](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)


prototype과 class를 비교하며
어떻게 활용하는지는 다음 포스트에서 확인할 수 있다

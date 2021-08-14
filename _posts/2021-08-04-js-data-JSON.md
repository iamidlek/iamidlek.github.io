---
title: js-data & JSON
author: YHole
date: 2021-08-04 +0900
categories: [프론트 엔드, js]
tags: [js, data, Shallow copy, Deep copy, 얕은 복사, 깊은 복사, import, JSON]
---

## 표준 내장 객체

[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects)문서에서 각 객체와 활용법에 관하여 확인 할 수 있다

확인 방법의 예시

### 예시 1

##### Number

- const x = parseInt(str)
- 문자 데이터를 정수 데이터로 변환하여 반환

### 예시 2

##### Object

- const copy = Object.assign({}, 객체1, 객체2)
- 첫 번째 인자에 삽입할 배열 지정
- 두 번째 이후는 합칠 객체들을 넣는다
- .prototype. 이 붙지않은 경우 정적 메서드라고 한다
- {}.메서드 로 사용할 수 없다 (Object 라는 전역 객체를 사용)


### 예시 3

#### Spread

- 배열이나 문자열과 같이 반복 가능한 문자를 확장

```javascript
const abc = ['a', 'b', 'c']
console.log(...abc)
// a b c

function test(a, ...b){
  return {
    a,
    b
  }
}

test(...abc)
// { a: 'a', b: [ 'b', 'c' ] }
```

> 종류가 다양하기 때문에 자주 사용하지 않는 속성, 메서드들은
> 한 번에 외우는 것이 아닌 대략적 파악 후 사용하면서 익힌다

## Shallow copy & Deep copy

### 원 시데이터 참조형 데이터

얕은 복사와 깊은 복사를 알려면 원시데이터에 대하여 알아야 한다

- 불변성 (Immutability)
  - 원시 데이터: String, Number, Boolean, undefined, null
  - 값이 같다면 같은 주소를 가지고 있다

- 참조형 데이터 : Object, Array, Function
  - 같은 값 또는 내용이 들어있어도 각각 선언하면 다른 주소를 가진다
  - 변수간 할당을 하면 주소가 같아진다

### 얕은 복사(Shallow copy)

- 맨 껍질 부분이 `다른 주소`를 가진다
- { 1, 2, [], 4} 라면 {} 객체 자체는 `다른 주소`
- 배열이 가지는 주소는 원본과 복사본이 `같은 주소`를 가진다  

### 깊은 복사(Deep copy)

- 복합적인 객체를 복사하고 `내부도 재귀적으로` 복사
- 모두 다 다른 주소를 가지므로 서로 영향 없음

- 실제 구현은 lodash 라이브러리를 이용하면 편리하다
- import _ from 'lodash'
- const copyAbc = _.cloneDeep(abc)


## js 가져오기, 내보내기

- js 파일을 가져오는 것은 간단하다 import 이름 from 경로
- 내보내기는 2가지 통로가 있다

1. Default export
  - export default 함수 → 하나의 함수만 작성가능
  - 가져온 후 이름은 자유롭게 정해서 사용 가능
2. Named export
  - export function 함수명1 → 여러가지 함수 작성 가능  
  export function 함수명2 → 두번째 함수
  - import { 함수명1, 함수명2 as 변경할 이름 } from 경로 로 사용
  - import * as 이름 from 경로 로 다 가져올 수도 있다

- 이름이 있는 export 여러개와 Defalt export 하나를 조합하여 사용도 가능

## JSON

먼저 JSON파일을 어디서 먼저 보게 되는지 알아보자

### package.json

```bash
# package.json 생성
npm init -y

# parcel-bundler 를 -D(개발 환경에서만 사용)옵션으로 설치
# devDependencis 개발 의존성 웹브라우저에서 필요없는것
# -D 를 붙였을때는 개발할때만 도움을 받는 패키지 = --save-dev
npm install parcel-bundler -D

# dependens는  웹 브라우저에서 필요한 패키지
```
- 추가된 install 내역은 package.json에 자동으로 추가됨

> 노드 모듈스 안에 다른 패키지도 설치되는 이유?
> 파슬 번들러 실행할때 필요한 패키지가 따라서 설치됨

- package.json의 scripts 부분에  
"dev": "parcel index.html" 을 작성하면  
dev 라는 명령어로 개발 서버 사용이 가능하다

```bash
npm run dev
```

- lock 파일은 아까 처럼 연관된 패키지들이 전부 기록됨(자동이라 관리 x)

### JSON 사용

- 문자 데이터로 저장된다

```json
{
  "string": "yhole",
  "number": 13,
  "boolean": true,
  "null": null,
  "object": {},
  "array": []
}
```
- JSON에서 사용가능한 문자열 형태로 만들어줌  
const x = JSON.stringify(js객체)

- JSON내용을 js에서 사용가능한 현태로 만들어줌
const x = JSON.parse(json내용)



## 헷갈림 포인트

### 빌드 난독화
- 번들러는 결과를 낼때 사용한다
- 개발할때만 사용 
- 번들 개발 모듈들을 하나로 묶어낸것 


### 유의적 버전 semantic versioning
- 숫자가 의미하는것
- 메이저. 마이너. 패치

- 메이저 : 옛버전이랑 호환되지 않는 큰 변화
- 마이너 : 기존버전과 호환되는 새로운 기능이 추가된 버전
- 패치 : 기존 버전 호환 버그 오타 수정

- package.json에서 맨앞 ^이 붙어있으면  
메이저 버전은 바뀌지않고 마이너 패치 등이 바뀔수있음  
캐럿 기호가 없으면 업데이트 안됨

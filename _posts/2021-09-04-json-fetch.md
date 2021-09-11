---
title: JSON-가져오기
author: YHole
date: 2021-09-04 +0900
categories: [Front end, json]
tags: [convert, json, js]
---

## JSON 가져오기

- 로컬에서 사용하는 방법도 있지만 (import)  
일반적으로 주소(api)로 가져오는 것 같다


### json 올리기

- 개인이 간단하게 할 수 있는 방법으로 gist 사용
  - github gist로 검색하여 들어감
  - 파일 내용가 파일이름.json, 퍼블릭으로 저장
  - RAW를 눌러 주소를 긁어 온다


### 경우 1

```js
 state.bible = []
  const endpoint = 'https://gist.githubusercontent.com/iamidlek/08a5d8759ff657352c3cc6418c635590/raw/ea99fa8105dbe8e6759c9aff54b34d5abdff801a/en_en.json';
  fetch(endpoint)
  .then(blob => blob.json())
  .then(data => state.bible.push(...data));
```

- 주소를 fetch('주소') 로 사용해도 무방하다
- .then 으로 가져와 지면 실행 할 동작을 설정한다
- .json()으로 클라이언트가 사용 가능한 json 형식으로으로 변환
- 위의 예의 경우 미리 준비한 배열에 넣어준다
- 주의 
  - [{}, {}, {} ...] 과 같은 구조의 json일 경우  
  [... data] 스프레드로 작성해서 배열로 넣어주면 [{}, {}, {} ...] 그대로 유지됨


### 경우 2

- 아래의 예는 다른 구조이므로 다른 방식을 사용한다
  - { "name": [{}, {}, {} ...]}

```js
fetch('https://gist.githubusercontent.com/iamidlek/f875ad6f59d5afe1e232a01287b40164/raw/923194a341dbdbdfdf77947bd1a77cd823a8b0aa/cashbook.json')
  .then(blob => blob.json())
  .then(data => value(data))

const expenses = []

// 프로미스 결과를 빼내오기위한 함수
function value (data) {
  // name을 불러서 
  data.name.forEach(element => {
    expenses.push(element)
		// 테스트
    console.log(element.date)
  });
}
```



### 메모

- 프로미스 객체로 받아온 내용을  
그냥 배열 처럼 사용하려고 해서 매우 힘들었다
- 함수를 이용해서 정의된 변수에 담아서 사용하고  
배열에 담아서 사용하는 것이 편리한 것 같다
- 함수 내에서 promise 객체를 이용해야한다는 것을 알았다

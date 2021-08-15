---
title: clone-starbucks-1
author: YHole
date: 2021-08-05 +0900
categories: [Front end, practical exercise]
tags: [clone, 클론 코딩, starbucks, 실습]
---

## clone 한 내용

> 최종 결과 메인 페이지, 로그인 페이지

- Header
  - 검색 바
  - 메뉴 바
  - 메뉴 

## 고민한 부분

- 검색 바
  - 검색 바에 아이콘 넣기
  - focus 되었을때 placeholder 보이기
  - google fonts에서 아이콘 넣기
  - focus, blur 이벤트 처리

- 메뉴 바와 메뉴
  - 메뉴 바의 아이템에 마우스 올리면 메뉴 보이기
  - 메뉴 바 고정
  - 스크롤 시에도 상단에 고정되어 같은 기능 유지

## 알게된 부분

- 검색 바

```html
<!-- input을 한 번 더 감싸서 아이콘 추가 -->
<div class="search">
  <input type="text" />
  <span class="material-icons">search</span>
</div>
```
```javascript
// 이벤트 리스너를 사용
// 속성과 값을 지정해 줄 수 있다
serchInputEl.addEventListener('focus', function(){
  searchEl.classList.add('focused');
  serchInputEl.setAttribute('placeholder', '통합검색');
});

serchInputEl.addEventListener('blur', function(){
  searchEl.classList.remove('focused');
  serchInputEl.setAttribute('placeholder', '');
});
```
- 메뉴 바와 메뉴

```css
/* 항상 화면 상단에 고정 */
/* 같은 형제 요소중 상단에 위치 */
header {
  position: fixed;
  top:0;
  z-index: 9;
}
```

## 헷갈림 포인트

- 구현에 있어서의 고민
  - 어느 부분까지 html로 구현?
  - 어느 효과까지 css로 구현?
  - 사이트가 무거워지지 않을 정도의 js?

- 이벤트 리스너에는 어떤 종류가 있을까?
  - 이벤트 핸들링 [MDN](https://developer.mozilla.org/ko/docs/Web/Events)문서를 읽어보자

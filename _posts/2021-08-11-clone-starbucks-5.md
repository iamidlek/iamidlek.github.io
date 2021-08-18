---
title: clone-starbucks-5
author: YHole
date: 2021-08-11 +0900
categories: [Front end, practical exercise]
tags: [clone, starbucks, practical]
---

## clone 내용

> 최종 결과 메인 페이지, 로그인 페이지

- 완료
  - HEADER
  - VISUAL
  - NOTICE
  - REWARDS
  - YOUTUBE VIDEO
  - SEASON PRODUCT
  - RESERVE COFFEE
  - PICK YOUR FAVORITE
  - RESERVE STORE
  - FIND THE STORE
  - AWARDS
  - FOOTER

- 진행
  - 배지, 탑 버튼
  - ScrollMagic
  - Copyright 날짜
  - signin page

## 고민한 부분

- 배지, 탑 버튼
  - 스크롤에도 따라 올 것
  - 일정 위치에서 사라지고 나타날것
  - 탑 버튼을 누르면 맨 위로 스크롤

- ScrollMagic
  - 일정한 위치에 스크롤이 되는 것을 감지

- Copyright 날짜
  - 수정을 안해도 자동으로 최신 년도로 갱신

- signin page
  - html 연결

## 알게된 부분

### 배지, 탑 버튼

- 두 개의 라이브러리 사용
  - lodash 라이브러리
  - gsap 라이브러리

```javascript
const badgeEl = document.querySelector('header .badges');
const toTopEl = document.querySelector('#to-top');

window.addEventListener('scroll', _.throttle(function (){
  console.log(window.scrollY);
  // 스크롤 위치
  if (window.scrollY > 500){
    // 배지 숨기기
    // gsap.to(요소,지속시간, 옵션)
    gsap.to(badgeEl, .6,{
      opacity: 0,
      display: 'none'
    });
    //  탑 버튼 보이기
    gsap.to(toTopEl, .2 ,{
      x: 0 
    });
  } else {
    // 배지 보이기
    gsap.to(badgeEl, .6,{
      opacity: 1,
      display: 'block'
    });
    // 탑 버튼 숨기기
    gsap.to(toTopEl, .2 ,{
      x: 100 
    });
  }
},300));
```

- ScrollToPlugin

```javascript
// 톱페이지로 올라가기
toTopEl.addEventListener('click', function () {
  // 페이지 위치를 최상단으로 부드럽게(0.7초 동안) 이동.
  gsap.to(window, .7, {
    scrollTo: 0 //0px ... gsap의 플러그인 추가로 필요한부분
  });
});
```


### ScrollMagic

- 특정 요소의 특정 위치를 지나는 것을 감시

```javascript
const spyEls = document.querySelectorAll('section.scroll-spy');
spyEls.forEach(function (spyEl){
  new ScrollMagic
    .Scene({
      triggerElement: spyEl, // 보여짐 여부를 감시할 요소를 감시
      triggerHook: .8 // 뷰포트의 가운데 가 0.5  0.8 지날때 트리거
    })
    .setClassToggle(spyEl,'show') // 트리거가 걸리면 실행
    .addTo(new ScrollMagic.Controller());
});
```

- show 클래스가 부여되면  
보여 져야할 위치보다 좌우로 떨어져있다가  
원위치로 돌아오는 애니메이션 구현

```css
.back-to-position{
  opacity: 0;
  transition: 1s;
}
.back-to-position.to-right{
  transform: translateX(-150px);
}
.back-to-position.to-left{
  transform: translateX(150px);
}
.show .back-to-position{
  opacity: 1;
  transform: translateX(0);
}
```

### Copyright 날짜

- 연도 불러오기

```javascript
const thisYear = document.querySelector('.this-year');
thisYear.textContent = new Date().getFullYear();
```

### signin page

- signin 페이지를 구현한 경로를 입력
- 클릭시 페이지 이동

```html
<a href="/signin/index.html">Sign In</a>
```

## 마치며

- js, css를 common과 각 기능별로 작성하여 재사용이 용이하게 하기

- 다양한 라이브러리를 찾아보기

- html에서 설정할 부분과 css, js에서 설정할 부분을 잘 구분하기

- 실제로 개인적인 클론코딩을 통해 스스로 사용 가능하도록 연습하기

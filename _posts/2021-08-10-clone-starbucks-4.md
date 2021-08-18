---
title: clone-starbucks-4
author: YHole
date: 2021-08-10 +0900
categories: [Front end, practical exercise]
tags: [clone, starbucks, practical]
---

## clone 내용

> 최종 결과 메인 페이지, 로그인 페이지

- 완료
  - HEADER 영역
  - VISUAL 영역
  - NOTICE 영역
  - REWARDS 영역
  - YOUTUBE VIDEO 영역
  - SEASON PRODUCT 영역
  - RESERVE COFFEE 영역

- 진행

  - PICK YOUR FAVORITE
  - RESERVE STORE
  - FIND THE STORE
  - AWARDS
  - FOOTER

## 고민한 부분

- PICK YOUR FAVORITE
  - 고정되어 스크롤 안되는 배경

- RESERVE STORE
  - 입체 메달 구현

- FIND THE STORE
  - 특별히 없음

- AWARDS
  - 슬라이드 복습

- FOOTER
  - 특별히 없음

## 알게된 부분

### PICK YOUR FAVORITE

- Parallax Scrolling

```css
.pick-your-favorite {
  background-image: url(../images/favorite_bg.jpg);
  background-repeat: no-repeat;
  background-position: center;
  /* 요소 고정 스크롤시 이미지가 그냥 그대로 패럴렉스*/
  background-attachment: fixed; 
  background-size: cover;
}
```

### RESERVE STORE

- 입체적 변화
  - 부모 요소에 perspective
  - 자식 요소 transform: rotateY
  - backface-visibility 뒷면 보이기 설정

```css
.reserve-store .medal {
  width: 334px;
  height: 334px;
  perspective: 600px; /* 원근감 부모에 */
}
.reserve-store .medal .front,
.reserve-store .medal .back {
  width: 334px;
  height: 334px;
  backface-visibility: hidden;
  transition: 1s;
  position: absolute;
}
/* 앞면 */
.reserve-store .medal .front{
  transform: rotateY(0deg);
}
.reserve-store .medal:hover .front{
  transform: rotateY(180deg);
}
/* 뒷면 */
.reserve-store .medal .back{
  transform: rotateY(-180deg);
}
.reserve-store .medal:hover .back{
  transform: rotateY(0deg);
}
```

### AWARDS

- 슬라이드 구현
  - Swiper 라이브러리 이용

```javascript
new Swiper('.awards .swiper-container', {
  autoplay: true,
  loop: true,
  spaceBetween: 30,
  slidesPerView: 5,
  navigation: {
    prevEl:'.awards .swiper-prev',
    nextEl:'.awards .swiper-next'
  }
});
```

## 헷갈림 포인트

- Parallax Scrolling과 같은 디자인적인 기법도 알아두면 좋겠다
- transform등 css를 이용한 구현에 익숙해질 필요가 있다

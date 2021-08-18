---
title: clone-starbucks-3
author: YHole
date: 2021-08-09 +0900
categories: [Front end, practical exercise]
tags: [clone, starbucks, practical]
---

## clone 내용

> 최종 결과 메인 페이지, 로그인 페이지

- 완료
  - HEADER 영역
  - VISUAL 영역
  - NOTICE 영역

- 진행
  - REWARDS
  - YOUTUBE VIDEO
  - SEASON PRODUCT
  - RESERVE COFFEE

## 고민한 부분

- REWARDS 영역
  - 특별히 없음
  - 버튼 스타일만 적용


- YOUTUBE VIDEO 영역
  - 영상 넣기
  - 그 위에 떠있는 느낌의 이미지 넣기

- SEASON PRODUCT, RESERVE COFFEE
  - 특별히 없음

## 알게된 부분

### YOUTUBE VIDEO 영역

- 프레임 크기, 위치
- 영상API
- 영상 옵션

```html
<!-- 클래스는 이하와 같다 -->
<div id="player">
```

```css
/* 계산도 가능 */
.youtube .youtube__area {
  width: 1920px;
  position: absolute;
  /* 가운데 정렬 방법 */
  left: 50%;
  margin-left: calc(1920px / -2);
  top: 50%;
  /* 16:9 비율 */
  margin-top: calc(1920px * 9 / 16 / -2);
}
```


```javascript
 // 2. This code loads the IFrame Player API code asynchronously.
 var tag = document.createElement('script');

 tag.src = "https://www.youtube.com/iframe_api";
 var firstScriptTag = document.getElementsByTagName('script')[0];
 firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

 // 3. This function creates an <iframe> (and YouTube player)
 //    after the API code downloads.

 function onYouTubeIframeAPIReady() {
  new YT.Player('player', {
     videoId: 'An6LvWQuj_8', // 최초 재생할 유튜브 영상 ID
     playerVars: {
       autoplay: true,
       loop: true,
       playlist: 'An6LvWQuj_8'
     },
     events: {
       onReady: function (event) {
        event.target.mute()
       }
     }
   })
 }
```

- 떠있는 느낌의 이미지 넣기

```javascript
// 함수 생성
// 랜덤한 딜레이시간을 리턴하는 함수
function random(min, max){
  return parseFloat((Math.random() * (max - min) + min).toFixed(2));
};

// 요소와, 딜레이 max 시간, y축 이동거리를 인자로 넣어준다
function floatingObject (selector,delay,size) {
  // gsap.to(요소,시간, 옵션);
  gsap.to(selector, random(1.5,2.5), {
    // y축 이동
    y: size,
    repeat: -1, //무한 반복
    yoyo: true,
    ease: Power1.easeInOut,
    // 랜덤 함수 호출
    delay: random(0, delay)
  });
};

floatingObject('.floating1', 1, 15);
floatingObject('.floating2', .5, 15);
floatingObject('.floating3', 1.5, 20);
```

## 헷갈림 포인트

- youtube iframe api에 관해서 더 알아보아야겠다
- html 구조를 잘 구성해야 css, js의 효과를 부여하기  
용이하겠다고 느꼈다

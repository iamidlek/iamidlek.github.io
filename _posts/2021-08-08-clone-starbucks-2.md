---
title: clone-starbucks-2
author: YHole
date: 2021-08-08 +0900
categories: [Front end, practical exercise]
tags: [clone, starbucks, practical]
---

## clone 내용

> 최종 결과 메인 페이지, 로그인 페이지

- 완료
  - HEADER 영역

- 진행
  - VISUAL 영역
  - NOTICE 영역
 

## 고민한 부분

- VISUAL 영역
  - 이미지와 글씨의 위치 잡는 부분
  - 순차적으로 이미지 보이게하기


- NOTICE 영역
  - 공지 사항 슬라이드
    - 세로로 자동 슬라이드, 무한 루프 슬라이드
  - 클릭 시 확장되며 보이는 프로모션 영역
    - 세로 길이가 늘어나며 보여짐
  - 프로모션 영역 내부의 슬라이드 구현
    - 3 장의 슬라이드, 슬라이드 버튼, 가운데 포커스

## 알게된 부분

### VISUAL 영역
  - forEach 활용
  - 순차적 효과를 위해 index 활용
  - 동일한 duration을 주고 delay로 순차적인 표현


```javascript
// fade-in 클래스가 있는 요소 전부 가져오기
const fadeEls = document.querySelectorAll('.visual .fade-in');

// forEach 두번째 파라미터 인덱스 번호 이용
fadeEls.forEach(function (fadeEl, index) {
  gsap.to(fadeEl,1,{
    // 인덱스에 따라 순차적으로 0.7 1.4 2.1 2.7 초에 실행된다!
    delay: (index + 1) * .7, // 0부터 4개의 요소(0~3)
    opacity:1
  });
});
```

- gsap 라이브러리 활용
  - cdn주소를 index.html에 입력

```html
<script 
src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.7.1/gsap.min.js" 
integrity="sha512-UxP+UhJaGRWuMG2YC6LPWYpFQnsSgnor0VUF3BHdD83PS/
pOpN+FYbZmrYN+ISX8jnvgVUciqP/fILOXDjZSwg==" 
crossorigin="anonymous" 
referrerpolicy="no-referrer">
</script>
```

### NOTICE 영역

- 공지사항 
  - SWIPER 라이브러리 사용
  - &lt;div class=&quot;swiper-container&quot;&gt;
	  - &lt;div class=&quot;swiper-wrapper&quot;&gt;
		  - &lt;div class=&quot;swiper-slide&quot;&gt;
  - 위의 구성으로 슬라이드 구현이 간단하게 완료
  

```html
<link rel="stylesheet" 
href="https://unpkg.com/swiper/swiper-bundle.min.css">
<script src="https://unpkg.com/swiper/swiper-bundle.min.js"></script>
```

```javascript
// 방향, 자동재생, 반복을 설정
new Swiper('.notice-line .swiper-container', {
  direction: 'vertical',
  autoplay: true,
  loop: true
});
```

- 프로모션 영역
  - css 영역에 클래스를 나누어 설정
  - transition 추가(높이에 대한)
  - js로 클릭시 해당 요소에 hide 클래스 부여

```javascript
// 클래스를 받을 요소
const promotionEl = document.querySelector('.promotion');
// 버튼 이벤트를 감지할 요소
const promotionToggleBtn = document.querySelector('.toggle-promotion');
// 상태 제어
let isHidePromotion = false;

promotionToggleBtn.addEventListener('click', function () {
  isHidePromotion = !isHidePromotion
  if (isHidePromotion){
    // 숨김 처리
    promotionEl.classList.add('hide');
  } else {
    // 보여주기 처리
    promotionEl.classList.remove('hide');
  }
});
```

```css
.notice .promotion {
  height: 693px;
  background-color: #f6f5ef;
  position: relative;
  transition: height .4s;
  overflow: hidden;
}
/* hide가 생겼을 경우 */
.notice .promotion.hide{
  height: 0;
} 
```

- 프로모션 영역 내부의 슬라이드
  - 라이브러리를 사용하여 여러 가지 옵션을 줄 수 있다

```javascript
new Swiper('.promotion .swiper-container', {
  slidesPerView: 3,// 한번에 보여지는 슬라이드 개수
  spaceBetween: 10,// 슬라이드 사이 여백
  centeredSlides: true, // 1번슬라이드가 가운데 포커스
  loop:true,
  autoplay:{
    delay: 5000 //5초
  },
  pagination:{
    el: '.promotion .swiper-pagination',//페이지 번호 요소 선택자
    clickable: true // 페이지 번호 요소 제어 가능 여부
  },
  navigation: { // 슬라이드 버튼 설정
    prevEl:'.promotion .swiper-prev',
    nextEl:'.promotion .swiper-next'
  }
});
```


## 헷갈림 포인트

- 현업에 있어서 적재적소에 라이브러리를 사용하면  
매우 강력할 것 같다
  - 라이브러리에 의존해도 되는 부분인가 궁금하다
  - 다음엔 슬라이드, 페이지네이션을 직접 구현해 보고싶다

- 다양한 라이브러리를 알아둘 수 있는 공부법(?)도 고민해 봐야겠다

- 클래스 부여와 transition
  - js로 요소에 클래스를 부여하여도 css의 transition이 적용되는 점

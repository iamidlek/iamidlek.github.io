---
title: clone-soylent-6
author: YHole
date: 2021-08-17 +0900
categories: [Front end, practical exercise]
tags: [clone, soylent, practical]
---

## clone 내용

![section](/assets/img/soylent/slide4.PNG)

## 파악하기

- 구현된 목록

- 4가지 탭을 선택할 수 있다
- 탭마다 다른 문구와 슬라이드가 보여진다
- 슬라이드는 3개씩 보여진다 
- 라디오 버튼 모양 만들기
- 라디오 버튼 클릭시 하단의 표시내용 변경
- 날짜 텀 드롭다운으로 선택 가능하게
- +,- 버튼, 숫자 5까지 선택 가능
- 페이지 버튼이 있으며 max 로 가면 클릭 불가 상태가 된다

- 구현해볼 목록

- [x] 페이지 네이션이 있으며 자체 클릭이 가능하다
- [ ] (클릭 + 드레그)로 슬라이드 넘기기 기능

## 구조 만들기

[clone-soylent-5](https://yhole.netlify.app/posts/clone-soylent-5/) 의 연속

## 스타일 만들기

[clone-soylent-5](https://yhole.netlify.app/posts/clone-soylent-5/) 의 연속

## Javascript

- 슬라이드 기능
- 페이지네이션 연동
- 실행중 중복 클릭 방지(라이브러리 이용)

```javascript
// 슬라이드 박스 별로 적용
for (let s = 1; s <= slideboxELs.length; s++) {
  const pageMax = document.querySelectorAll(`.sliderbox${s} .page`).length;
  const moveBox = document.querySelector(`.sliderbox${s} .move-box`);
  // 슬라이드 기능 필요 여부 확인
  if (pageMax == 1){
    const nonslide = document.querySelector(`.sliderbox${s} .slide-status`)
    nonslide.style.display = "none";
  } else {
    const Rarrow = document.querySelector(`.sliderbox${s} .right-arrow`);
    const Larrow = document.querySelector(`.sliderbox${s} .left-arrow`);
    const nationpage = document.querySelector(`.sliderbox${s} .nation-bar`);
    // const slideWidth = parseFloat(window.getComputedStyle(document.querySelector(`.sliderbox${s} .slide-bar`)).width)
    
    // 페이지 네이션 생성
    for (let n = 1; n <= pageMax; n++){
      Pgnt(nationpage,pageMax,n)
    }

    // 현 페이지 위치
    let currentpage = 1

    // 오른쪽으로 이동 버튼 클릭시
    Rarrow.addEventListener('click', _.debounce(function () {
      // 위치 파악
      let position = parseFloat(window.getComputedStyle(moveBox).left);
      currentpage += 1;
      let pagenation = document.querySelector(`.sliderbox${s} .nation${currentpage}`)
      for (let n = 1; n <= pageMax; n++){
        let removenation = document.querySelector(`.sliderbox${s} .nation${n}`)
        removenation.classList.remove('nowhere')
      };
      pagenation.classList.add('nowhere');
      // 끝까지 이동했는지 확인
      if (currentpage == pageMax) {
        Rarrow.classList.add('maxed');
        Larrow.classList.remove('maxed');
        moveBox.style.transition = "left " + 200 + "ms";
        moveBox.style.left = (position - 1320) + "px";
      } else {
        Larrow.classList.remove('maxed');
        Rarrow.classList.remove('maxed');
        moveBox.style.transition = "left " + 200 + "ms";
        moveBox.style.left = (position - 1320) + "px";
      }
    }, 210));

    // 왼쪽으로 이동 버튼 클릭시
    Larrow.addEventListener('click', _.debounce(function () {
      let position = parseInt(window.getComputedStyle(moveBox).left);
      currentpage -= 1;
      let pagenation = document.querySelector(`.sliderbox${s} .nation${currentpage}`)
      for (let n = 1; n <= pageMax; n++){
        let removenation = document.querySelector(`.sliderbox${s} .nation${n}`)
        removenation.classList.remove('nowhere')
      };
      pagenation.classList.add('nowhere');
      if (currentpage == 1) {
        Larrow.classList.add('maxed');
        Rarrow.classList.remove('maxed');
        moveBox.style.transition = "left " + 200 + "ms";
        moveBox.style.left = (position + 1320) + "px";
        
      } else {
        Larrow.classList.remove('maxed');
        Rarrow.classList.remove('maxed');
        moveBox.style.transition = "left " + 200 + "ms";
        moveBox.style.left = (position + 1320) + "px";
      }
    }, 210));

    // 페이지 네이션 클릭시 (연동)
    for (let n = 1; n <= pageMax; n++){
      const nowhe = document.querySelector(`.sliderbox${s} .nation${n}`);
      nowhe.addEventListener('click',function(){
        for (let w = 1; w <= pageMax; w++){
          let removhere = document.querySelector(`.sliderbox${s} .nation${w}`)
          removhere.classList.remove('nowhere')
        };
        nowhe.classList.add('nowhere')
        moveBox.style.left = (-1320 * (n-1)) + "px";
        currentpage = n
        if (n == pageMax){
          Rarrow.classList.add('maxed');
          Larrow.classList.remove('maxed');
        } else if (n == 1){
          Larrow.classList.add('maxed');
          Rarrow.classList.remove('maxed')
        } else {
          Larrow.classList.remove('maxed');
          Rarrow.classList.remove('maxed');
        }
      });
    }
  }
}
```

## 느낀점

- 너무 장대한 코드가 되어버린 것 같다
- 실무에서 이런 코드가 사용될 것 같지는 않다
- 의도한 동작이 제대로 문제없이 구동되어서 신기하다

- 슬라이드 전용 라이브러리를 사용하지 않고 구현하여  
많은 문제에 봉착하고 검색하고 해결하는 경험을 하였다
- 분명 더 좋은 방법이 생각날 것만 같다

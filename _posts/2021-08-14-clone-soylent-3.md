---
title: clone-soylent-3
author: YHole
date: 2021-08-14 +0900
categories: [Front end, practical exercise]
tags: [clone, soylent, practical]
---

## clone 내용

![section](/assets/img/soylent/slide1.PNG)

## 파악하기

- 구현된 목록

- 구현해볼 목록

- [x] 4가지 탭을 선택할 수 있다
- [x] 탭마다 다른 문구와 슬라이드가 보여진다
- [x] 슬라이드는 3개씩 보여진다
- [ ] 페이지 네이션이 있으며 자체 클릭이 가능하다
- [ ] 페이지 버튼이 있으며 max 로 가면 클릭 불가 상태가 된다
- [ ] 라디오 버튼 모양 만들기
- [ ] 라디오 버튼 클릭시 하단의 표시내용 변경
- [ ] 날짜 텀 드롭다운으로 선택 가능하게
- [ ] + - 버튼, 숫자 5까지 선택 가능
- [ ] 클릭 + 드레그로 슬라이드 넘기기 기능

## 구조 만들기

- container, inner 안
  - h1
  - 슬라이드 디스플레이 영역

- 슬라이드 디스플레이 영역
  - 4 가지 탭을 나타낼 영역
  - 슬라이드 전체 박스

- 슬라이드 전체 박스(1,2,3,4 탭)
  - slider 영역

- slider 영역
  - 움직이는 박스
    - 페이지(1,2,3....)
      - 슬라이드 1,2,3

## 스타일 만들기

### 슬라이드 디스플레이영역

> 코드 보다 구현 아이디어가 더 중요하였다

- 각 탭별로 나올 슬라이드 박스를 준비
- 슬라이드 박스 내부에는 슬라이더로 영역을 잡아준다
- 슬라이더 내부에는 page로 움직일 대상을 만든다
- page 내부에는 슬라이드가 3개씩 들어간다

![section](/assets/img/soylent/slide1-1.PNG)


## Javascript

- 탭 클릭시 언더라인을 녹색으로
- 탭이 눌린것을 확인하여 해당 슬라이더 박스 보여주기

```javascript
const tabEls = document.querySelectorAll('.display-tab ul li');
const slideboxELs = document.querySelectorAll('.sliderbox');

// clicked 클래스가 있으면 언더라인 녹색효과(css)
for(let i=0;i<tabEls.length;i++){
  tabEls[i].addEventListener('mousedown',function () {
    for(let m = 0; m<tabEls.length; m++){
      tabEls[m].classList.remove('clicked');
    };
    tabEls[i].classList.add('clicked');
  });
};

// 해당 탭에 clicked 가 있으면 슬라이더 박스 가 보여진다
// 탭과 슬라이드 박스를 인덱스로 같이 동작하도록 하였다
for(let i=0;i<tabEls.length;i++){
  tabEls[i].addEventListener('click',function () {
    for(let p=0;p<slideboxELs.length;p++){
      slideboxELs[p].classList.remove('active');
    }
    slideboxELs[i].classList.add('active');
  });
};
```

## 느낀점

- 브라우저에서 무언가 이벤트를 감지하여  
어떠한 요소를 움직이게하는 경우 js가 적절한 것 같다
- for 문으로 중복 코드를 쉽게 작성할 수 있다
- 아직 함수들을 잘 사용하지 못하겠다

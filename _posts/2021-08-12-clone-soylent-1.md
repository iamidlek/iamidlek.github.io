---
title: clone-soylent-1
author: YHole
date: 2021-08-12 +0900
categories: [Front end, practical exercise]
tags: [clone, soylent, practical]
---

## clone 내용

![header](/assets/img/soylent/header.PNG)

## 파악하기

- 최상단의 공지 부분
  - 스크롤을 내리면 사라짐

- 하단의 로고, 네비, 언어 선택, 로그인의 영역
  - fix되어 스크롤 시에도 따라옴

- 스크롤 시
  - 최상단 공지 부분이 사라짐
  - 사라진 부분만큼 하단의 헤더가 올라감
  - tpo 0 에 fix됨

- hover 시
  - 메뉴 옆의 드롭다운 표시가 반전됨
  - 메뉴 밑에 녹색 언더라인이 나타남
  - 메뉴의 상세가 드롭다운 됨
  - 언어 영역의 언어 선택지가 드롭다운 됨

- 메뉴 클릭시
  - 메뉴를 클릭하면 녹색으로 글씨색 변경
  - 다른 메뉴를 클릭하면 기존 녹색 글씨는 없어짐
  - 다른 선택된 메뉴에 녹색 글씨가 켜짐

## 구조 만들기

- header
  - notice banner 공지 사항
  - main header fix될 영역

- main header
  - wrapper1,2,3 드롭다운시 보여질 메뉴 상세
  - header bar 항상 보여질 헤더 메뉴

- header bar
  - logo 영역
  - 서브 영역
  - 메인 네비 영역

- 서브 영역
  - STORE LOCATOR
  - country-flag
    - 기본 언어
    - 선택지 드롭다운 (숨겨놓기)
  - 로그인, 쇼핑내역 아이콘

- 메인 네비 영역
  - 드롭다운 아이콘 (구글 아이콘)
  - 메뉴 이름

## 스타일 만들기

### z-index

- html의 구조에 영향을 많이 받는다
- position이 static이 아닌 같은 형제 요소간에 적용된다
- -1로 숨겼다 어떤 이벤트시에 양수로 변경하여 보여주기

> 드롭다운메뉴가 다음 영역을 가리게 해야하는 경우  
> 헤더가 스크롤되도 항상 위에 있게 해야하는 경우

### transition

- 부모에 주기
  - duration을 설정하면 진행과 역진행이 일어난다
  - 커지는 효과라면 커졌다 작아졌다 부드럽게 일어남

- 자식에 주기
  - duration을 설정하면 진행만 일어난다
  - 커지는 효과라면 커지는 것은 부드럽게 일어남
  - 작아지는 경우는 진행 시간 없이 작아짐

> soylent 사이트를 보면 메뉴에 hover해서 드롭다운 메뉴 상세가 보여진다  
> 보여질 때는 상단에서 스르륵 내려오는 효과가 있지만  
> 사라질 때에는 아무 효과없이 순식간에 없어진다

### display

- transition의 duration을 줄 때
  - display: none 에서 변경하면 효과가 나오지 않는다

- opacity를 0에서 1로 변경하는 방법을 사용하여 해결

### 메인 메뉴들 hover

- content: ''
  - before,after로 하나 만들어서 길이를 주고 색상을 준다
  - 메뉴에 hover 되면 보이게 함으로 밑줄을 생성할 수 있다


## Javascript

### 스크롤시 사라지는 notion banner

- lodash, gsap 라이브러리 사용
- 스크롤시 메인 헤더의 위치를 화면의 상단으로 변화시킴
- 자연스럽게 notice banner 위로 위차 하게 된다

```javascript
window.addEventListener('scroll', _.throttle(function (){
  if (window.scrollY <= 5){
    gsap.to(mainHeaderEl, 0.1,{
      top: '60px',
    });
  } else if (window.scrollY > 5 && window.scrollY <= 40){
    gsap.to(mainHeaderEl, 0.1,{
      top: '35px',
    });
  } else if (window.scrollY > 40 && window.scrollY <= 58){
    gsap.to(mainHeaderEl, 0.1,{
      top: '15px',
    });  
  } else if (window.scrollY > 58){
    gsap.to(mainHeaderEl, 0.1,{
      top: '0',
    });
  }
},10));
```

### 클릭된 메뉴만 글씨 색상 변경

- 클릭되면 녹색으로 글씨 색상이 바뀜
- 다른 메뉴를 누를 때까지 유지됨
- 다른 메뉴를 선택하면 효과가 사라짐 (단일 선택)

```javascript
for(let i = 0; i < pEls.length; i++){
  pEls[i].addEventListener('mousedown', function () {
    for(let r = 0; r < pEls.length; r++){
      if (pEls[r].classList.contains('mousedowneff')){
        pEls[r].classList.remove('mousedowneff')
       };
    }
    pEl6.classList.remove('mousedowneff');
    pEls[i].classList.add('mousedowneff');
  });
};
```

### 마우스 올리기 마우스 나가기

- mouseover, mouseout 이벤트를 이용
- 메뉴 상세 부분과 같이 hover/out 시에 작동해야 하는 경우 이용

- 또한 메뉴 상세 영역 위로 마우스가 올라갔을 때  
지속적으로 그 영역을 보여주어야 할 경우


```javascript
btnlearnEls.addEventListener('mouseover',function () {
  wrap3El.classList.add('hovered')
});
btnlearnEls.addEventListener('mouseout',function () {
  wrap3El.classList.remove('hovered')
```


## 느낀점

- html구조를 잘 짜지 않으면 효과주기가 굉자히 어렵다
- css의 효과가 생각보다 마음대로 설정되지 않는다
- js 아직 익숙하지 않아서 코드 만들기가 힘들다

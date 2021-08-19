---
title: clone-soylent-5
author: YHole
date: 2021-08-16 +0900
categories: [Front end, practical exercise]
tags: [clone, soylent, practical]
---

## clone 내용

![section](/assets/img/soylent/slide3.PNG)

## 파악하기

- 구현된 목록

- 4가지 탭을 선택할 수 있다
- 탭마다 다른 문구와 슬라이드가 보여진다
- 슬라이드는 3개씩 보여진다 
- 라디오 버튼 모양 만들기
- 라디오 버튼 클릭시 하단의 표시내용 변경
- 날짜 텀 드롭다운으로 선택 가능하게
- +,- 버튼, 숫자 5까지 선택 가능

- 구현해볼 목록

- [ ] 페이지 네이션이 있으며 자체 클릭이 가능하다
- [x] 페이지 버튼이 있으며 max 로 가면 클릭 불가 상태가 된다
- [ ] (클릭 + 드레그)로 슬라이드 넘기기 기능

## 구조 만들기

- sliderbox (1,2,3,4)
  - footslide
    - slide-status

- slide status
  - slide bar
  - arrow right
  - arrow left

## 스타일 만들기

### 화살표

- 이전 움직이는 화살표를 구현한 것과 같은 방법

### 페이지 네이션 스타일

- page 수로 길이를 부여하는 방식
- nowhere 클래스로 현위치 보여줌
- 포인터를 주어 선택 가능함을 알려줌

```css
.slide-bar .nation-bar{
  width: 100%;
  height: 0.4px;
  display: flex;
  position: absolute;
  left: 0;
  background-color: transparent;
}
.slide-bar .nation-bar li{
  position: relative;
  list-style: none;
}
.slide-bar .nation-bar li a{
  display: block;
  height: 100%;
  background-color: #d3d3d3;
  /* -webkit-transition: all .3s linear;
  -moz-transition: all .3s linear;
  -o-transition: all .3s linear; */
  transition: all .3s linear;
  cursor: pointer;
}
.slide-bar .nation-bar li a.nowhere{
  background-color: #181A19;
}
```

### max면 선택 안되게

- maxed 클래스로 관리

```css
.slide-status .maxed {
  pointer-events: none;
}
```

## Javascript

- 페이지 네이션을 구현하기 위한 함수
- 각 슬라이드별 page 수는 다를 수 있기 때문

```javascript
function Pgnt(parent,pageMax,n){
  const li = document.createElement("li")
  const a = document.createElement("a");
  a.setAttribute("class", `nation${n}`);
  a.style.width = (1190.34 / pageMax) + "px";
  if (n===1){
    a.setAttribute("class", `nation${n} nowhere`);
  }
  li.append(a)
  parent.appendChild(li);
}
```

## 느낀점

- 가로 길이 100%로 했는데 개발자 도구의 computed로 보면 달라짐  
- 고정 값을 하드코딩하면 안 좋지만 해결하지 못한 부분

- 다만 어떤 기능을 처음부터 구현하는 것에 있어서  
두려움은 많이 없어진 것 같다

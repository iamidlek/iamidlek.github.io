---
title: clone-soylent-7
author: YHole
date: 2021-08-21 +0900
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
- 페이지 네이션이 있으며 자체 클릭이 가능하다

- 실패한 기능

- [ ] (클릭 + 드레그)로 슬라이드 넘기기 기능

## 실패 분석

이유는 이하와 같다

1. 최종적인 기능을 처음에 파악하지 못한 점
  - 슬라이드가 가진 기능을 발견할 때마다 덧붙여 수정
2. 구현 능력 부족
  - 단순히 어떻게 구현해야 할까를 모르는 부분

## 과정

- clone-soylent-6에서 페이지 네이션을 구현한 후  
우연히 본래의 사이트의 슬라이드를 만지다 기능 발견

### 발견한 기능

1. 슬라이드가 드래그 가능한 형태
2. 드래그로 슬라이드 페이지 넘기기가 가능
3. 드래그 거리 감지(일정 거리 드래그 해야 넘어감)
4. 드래그 속도 감지(빠르게 넘기면 넘어감)

### 구현을 위한 노력

```javascript

const movebox = document.getElementById('mydiv');
// 마우스 다운 이벤트리스너와 동일한 효과
movebox.onmousedown = function(event) {
  // 스크롤을 무시한 브라우저 페이지에서의 X에서 화면 좌측부터 대상의 처음 위치 값을 빼준다
  // clientX `마우스`가 클릭하는 위치
  // getBoundingClientRect() `요소`의 위치
  
  let shiftX = event.clientX - movebox.getBoundingClientRect().left;
  
  // pageX 도 마우스의 위치 스크롤을 포함한 전체 문서의기준
  
  moveAt(event.pageX);

  function moveAt(pageX) {
    movebox.style.left = pageX - shiftX + 'px';
  }
// 그래픽을 고려하면 이하의 코드가 좋다고도 한다
// .transform = `translate(${x}px, ${y}px)`
  function onMouseMove(event) {
    moveAt(event.pageX);
  }

  document.addEventListener('mousemove', onMouseMove);

  movebox.onmouseup = function() {
    document.removeEventListener('mousemove', onMouseMove);
    movebox.onmouseup = null;
  };
  movebox.onmouseout = function() {
    document.removeEventListener('mousemove', onMouseMove);
    movebox.onmouseout = null;
  }
};

movebox.ondragstart = function() {
  return false;
};
```

### 정리

> 가장 중요한 것은 모든 기능들을 미리 파악하고  
> 그에 맞는 구조와 function을 만들어야 한다는 것이다

- 위 코드를 통하여 요소를 클릭하여 드래그 가능하게 하는 것은 가능했다
  - 마우스 다운 → 마우스 무브
  - 마우스 업 또는 마우스 아웃 → 마우스 무브 삭제


- 구현 실패(포기)한 내용
  1. 일정 거리 드래그시 슬라이드 이동
    - 원래 offset을 이용하여 드래그 거리 계산해보려 시도
  2. 시간 + 거리로 드래그 속도 파악하여 빠른 드래그시 슬라이드 이동
    - 1번이 실패해서 시도 못해봄
  

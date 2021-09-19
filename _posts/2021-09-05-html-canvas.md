---
title: html-canvas
author: YHole
date: 2021-09-05 +0900
categories: [Front end, html]
tags: [js, html, canvas]
---

## canvas 태그 사용

- 그래픽을 이용해 그리는 태그
- js로 컨트롤
- width height 속성이 있음

## javascritp

- 도형의 색, 시작점 등등 직접 지정해야한다
- 코드를 통하여 여러 명령들, 속성들을 알게됨

```js
const canvas = document.querySelector('#draw')

    // 랜더링 컨텍스트의 그리기 함수들을 사용
    // getContext() 메서드는 렌더링 컨텍스트 타입을 지정
    const ctx = canvas.getContext('2d');

    // 브라우저 윈도우의 뷰포트 너비
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    // 도형의 윤곽선 색
    ctx.strokeStyle = '#BADA55';
    // 선들이 만나는 모서리의 모양
    ctx.lineJoin = 'round';
    // 선 끝의 모양
    ctx.lineCap = 'round';

    ctx.lineWidth = 100;

    let isDrawing = false;

    let lastX = 0;
    let lastY = 0;
    
    // mother-effing 위치에따른 색상 변경
    let hue = 0;
    // 두께 변화를 위한 조건
    let direction = true;

    // 색 혼압과 관련
    // ctx.globalCompositeOperation = 'multiply';

    function draw(e) {
      if(!isDrawing) return; // 안그리기
      
        // hue이용 색상변경
      ctx.strokeStyle = `hsl(${hue},100%, 50%)`;

      // 새로운 경로를 생성
      ctx.beginPath();
      // 펜을 x,y 좌표로 옮긴다 start from
      ctx.moveTo(lastX, lastY);
      // 현재 펜의 위치에서 x,y까지 선을 그린다 go to
      ctx.lineTo(e.offsetX,e.offsetY);
      // 윤곽선을 이용하여 도형을 그린다
      ctx.stroke();

      // lastX = e.offsetX
      // lastY = e.offsetY
      [lastX,lastY] = [e.offsetX,e.offsetY];
      hue++;
      if (hue >= 360) {
        hue = 0
      }

      // 라인 두께 변화
      if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
        direction = !direction;
      }
      if (direction) {
        ctx.lineWidth++;
      } else {
        ctx.lineWidth--;
      }

    }

    // 마우스 움직이면
    canvas.addEventListener('mousemove', draw);
    canvas.addEventListener('mousedown', (e) => {
      // draw가 실행되도록
      isDrawing = true;
      // 첫 위치 잡아주기
      [lastX,lastY] = [e.offsetX,e.offsetY];
    })
    // draw를 안그리기로 빠지게하는 방법
    canvas.addEventListener('mouseup', () => isDrawing = false)
    canvas.addEventListener('mouseout', () => isDrawing = false)
```

## 메모

canvas를 처음 사용해봄
그림판처럼 그려지는게 신기했다

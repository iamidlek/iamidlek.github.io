---
title: team project-bankapp-2
author: YHole
date: 2021-09-15 +0900
categories: [Front end, practical exercise]
tags: [team project, bankapp, practical]
---

## 요소 drag

### drag up

  ```js
  // B.드래그 업 (지출 상세) + 상세 페이지 높이 변화 주기
  acountPages.forEach( page => {
  const dragBtn = page.querySelector('.drag_btn')
  const dragTarget = page.querySelector('.bank_use_history') 
  // let currTargetY = dragTarget.getBoundingClientRect().top
  let currTargetY = dragTarget.offsetTop
  // 최대 최소 값 (고정)
  const maxY = dragBtn.getBoundingClientRect().top
  const minY = page.querySelector('.home_header').getBoundingClientRect().bottom 

  // 컨트롤러
  let isDown = false
  let eStartY = 0

  // + 지출 상세 페이지 높이 변화
  const changeBottom = page.querySelector('nav').getBoundingClientRect().top - 8
  const detailPage = page.querySelector('.use_history_detail')

  window.addEventListener('load', changeHeight)
  function changeHeight () {
    const changeTop = page.querySelector('.saving').getBoundingClientRect().bottom
    const changedHeight = changeBottom-changeTop
    detailPage.style.height = changedHeight + "px"
  }

  // 모바일
  dragBtn.addEventListener('touchstart', moveActivate)
  dragTarget.addEventListener('touchmove', moveAct)
  dragTarget.addEventListener('touchend', moveEnd)
  
  // 웹 
  dragBtn.addEventListener('mousedown', moveActivate)
  dragTarget.addEventListener('mousemove', moveAct)
  dragTarget.addEventListener('mouseup', moveEnd)
  dragTarget.addEventListener('mouseout', moveEnd)
  
  function moveActivate (e) {
    isDown = true
    if (e.type === 'mousedown') {
      eStartY = e.clientY
    } else if (e.type === 'touchstart'){
      eStartY = e.touches[0].clientY
    }
  }

  function moveAct (e) {
    if (isDown) {
      if (e.type === 'mousemove') {
        const distanceY = (eStartY - e.clientY)
        const movedY = currTargetY - distanceY
        if (movedY <= maxY && movedY >= minY) {
          dragTarget.style.top  = movedY + 'px';
        }
      } else if (e.type === 'touchmove'){
        
        const distanceY = (eStartY - e.touches[0].clientY)
        const movedY = currTargetY - distanceY
        if (movedY <= maxY && movedY >= minY) {
          dragTarget.style.top  = movedY + 'px';
        }
      }
      changeHeight()
    }
  }
  
  function moveEnd () {
    isDown = false;
    currTargetY = dragTarget.getBoundingClientRect().top
  }
  })
  ```

### 메모

요소가 이동할 수 있는 최대, 최소 Y 값을 구하고  
요소가 선택된 상태에서 움직임이 발생할 때를 감시  
getBoundingClientRect()로 뷰포트 기준으로 구하는 것이  
슬라이드기능을 고려하면 좋은것인지 모르겠다
요소의 top 값을 주어 실제 요소가 움직였지만
height나 다른 방법으로 구현하는 것도 방법인 것 같다

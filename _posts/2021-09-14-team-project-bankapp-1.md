---
title: team project-bankapp-1
author: YHole
date: 2021-09-14 +0900
categories: [Front end, practical exercise]
tags: [team project, bankapp, practical]
---

## input

### input type="range" 를 스타일링

  ```html
  <div class="bar">
    <input type="range">
  </div>
  ```

  ```css
  /* 바 부분 */
  .bar input{
    -webkit-appearance: none;
    width: 202px;
    height: 6px;
    background: #C4C4C4;
    border-radius: 3px;
    margin-bottom: 12px;
  }
  .bar input:focus{ 
    outline: none;
  }

  /* 컨트롤 버튼 설정 */
  input::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 4px; 
    height: 17px;
    background: #FF5F00;
    border-radius: 2px;
    cursor: pointer;
  }
  input::-moz-range-thumb{
    -webkit-appearance: none;
    width: 4px; 
    height: 17px;
    background: #FF5F00;
    border-radius: 2px;
    cursor: pointer;
  }
  ```

  ```js
  // input 태그를 긁어 오기
  const inputs = document.querySelectorAll('input[type="range"]')

  // 실행할 내용
  function inputUpdate () {
    // 넘어오는 이벤트 주체의 값 (range의 경우이므로 0~100(%) 중 하나의 값)
    const perVal = this.value
    // console.log(this.value)
    
    // slider-thumb(컨트롤 하는 버튼)의 값을 기준으로 전후 그라디언트
    this.style.background = `linear-gradient(to right, #FFDB4C 0%, #FFDB4C ${perVal}%, #C4C4C4 ${perVal}%, #C4C4C4 100%)`
  }

  // 공통
  inputs.forEach(input => input.addEventListener('change', inputUpdate))
  // 웹
  inputs.forEach(input => input.addEventListener('mousemove', inputUpdate))
  // 모바일
  inputs.forEach(input => input.addEventListener('touchmove', inputUpdate))
  ```

  
### 완성 코드

```js
// B. input & value
acountPages.forEach( page => {
  const input = page.querySelector('.set-val')
  const homeInput = page.querySelector('.onlyview')
  const stdAmount = page.querySelector('.set_budget > span')
  const spended = page.querySelector('.total')
  const recommend = page.querySelector('.calced > span')

  // 정보 받아오면 실행으로 변경 가능
  window.addEventListener('load', () => inputUpdate(input,homeInput,stdAmount,spended,recommend))
  // 값 변경시
  input.addEventListener('change', () => inputUpdate(input,homeInput,stdAmount,spended,recommend))
  // 모바일
  input.addEventListener('touchmove', () => inputUpdate(input,homeInput,stdAmount,spended,recommend))
  // 웹
  input.addEventListener('mousemove', () => inputUpdate(input,homeInput,stdAmount,spended,recommend))
  
})

function inputUpdate (input,homeInput,stdAmount,spended,recommend) {
  homeInput.value = input.value
  const valueToPer = input.value / 50000
  input.style.background = `linear-gradient(to right, #FFDB4C 0%, #FFDB4C ${valueToPer}%, #C4C4C4 ${valueToPer}%, #C4C4C4 100%)`
  homeInput.style.background = `linear-gradient(to right, #FFDB4C 0%, #FFDB4C ${valueToPer}%, #C4C4C4 ${valueToPer}%, #C4C4C4 100%)`
  stdAmount.innerHTML= `${numberWithCommas(input.value)}원`

  // 권고
  const result = input.value - parseInt(spended.innerHTML.replace(/,/g,''))
  
  if (result >= 0) {
    recommend.innerHTML =`D -${restDate} : ${numberWithCommas(result)}원 남음`
  } else {
    recommend.innerHTML =`D -${restDate} : ${numberWithCommas(Math.abs(result))}원 초과`
  }
}
```

### 메모

input의 배경 색을 그라디언트의 %로 관리하면  
자연스러운 range 가 표현 가능해진다

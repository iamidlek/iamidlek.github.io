---
title: team project-bankapp-5
author: YHole
date: 2021-09-18 +0900
categories: [Front end, practical exercise]
tags: [team project, bankapp, practical]
---

## JSON 데이터 처리

- promise를 .then으로 후속 처리

  ```js
  acountPages.forEach( page => {
  const ulMother = page.querySelector('.use_history_detail > ol')
  currMonth.then( logs => {
    
    const dayList = Array.from(new Set(logs.map(l => l.date))).reverse()
    const dayAccum = getDayListAccum()
    const detailList = dayList.map(day=> logs.filter(l => day === l.date))

    function getDayListAccum () {
      return dayList.map(day=>{
        let count = 0
        logs.forEach(l => {
          if (day === l.date && l.income === 'out') count += l.price;
        })
        return count
      })
    }
    
    const html = dayList.map( (day, i) => {
      return pulsEl(day,dayAccum[i],detailList[i])
    }).join('')

    ulMother.innerHTML = html
    })
  })

  function pulsEl (day,accum,list) {
  const date = parseInt(day.slice(8)) === today ? '오늘' : parseInt(day.slice(8)) === today - 1 ? '어제' : `${parseInt(day.slice(8))}일`
  
  const htmlH = `
    <div>
      <span>${date}</span>
      <span>${numberWithCommas(accum)}원 지출</span>
    </div>
    `
  
  const htmlC = list.map(l => {
    const {income,history,price} = l
    if (income === 'in') {
      return `
      <li>
        <span>${history}</span>
        <span class="${income}">+ ${numberWithCommas(price)}</span>
      </li>
      `
    } else {
      return `
      <li>
        <span>${history}</span>
        <span class="${income}">${numberWithCommas(price)}</span>
      </li>
      `
    }
  }).join('')

  return `<li>${htmlH}<ol>${htmlC}</ol></li>`
  }
  ```

### 메모

실제 사용 내역을 보면 지출이 없는 날도 있어서  
없는 날은 어떻게 할지 생각해야 했다.

상세 부분이 복잡한 구조로 되어있어서 innerHtml로  
추가된 내용을 배열에서 string으로 변환해서 덮어쓰기 하는 방식이  
좋은지 궁금하다

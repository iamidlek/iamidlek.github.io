---
title: HTML Table&Details
author: YHole
date: 2021-07-30 +0900
categories: [프론트 엔드, html]
tags: [html, semantic, table, details, element, tag]
---

# 표 만들기


```html
<table>
  <caption>Fruits</caption>
  <colgroup>
    <!-- 총 세개의 열이 생김  -->
    <col span="2" style="background-color: yellowgreen;">
    <col style="background-color: tomato;">
  </colgroup>
  <thead>
    <tr>
      <th>ID</th>
      <th>Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1234</td>
      <td>lingo</td>
      <td>￥80</td>
    </tr>
    <tr>
      <td>5678</td>
      <td>Banana</td>
      <td>￥98</td>
    </tr>
  </tbody>
</table>
```

## &lt;table&gt;

- 테이블을 만들 때 사용

## &lt;caption&gt;

- 열린 테이블 태그 바로 뒤에 사용하며, 테이블당 한번만 사용, 표의 제목을 설정

## &lt;colgroup&gt; - &lt;col /&gt;

- 표의 열들을 공통적으로 정의하는 &lt;col/&gt;과 그의 집합&lt;colgroup&gt;

    ```html
    span 속성으로 열의 개수를 지정
    <col span="2" style="background-color: yellowgreen;">;
    ```

## &lt;thead&gt; - &lt;tbody&gt; - &lt;tfoot&gt;

- 머리, 본문, 바닥을 의미적으로 지정해준다 (레이아웃, 스타일의 변화는 없다)

## &lt;tr&gt; - &lt;th&gt; - &lt;td&gt;

- 행 , 셀 그룹의 헤더, 셀

    &lt;th&gt;는 속성 scope를 통해 가로헤더 세로 헤더를 지정한다

    ```html
    <table>
        <caption>Alien football stars</caption>
        <tr>
    				<!-- 행 생성, 행의 값들이 헤더 -->
            <th scope="col">Player</th>
            <th scope="col">Gloobles</th>
            <th scope="col">Za'taak</th>
        </tr>
        <tr>
    				<!-- 행 생성, 1열의 값만 헤더-->
            <th scope="row">TR-7</th>
            <td>7</td>
            <td>4,569</td>
        </tr>
        <tr>
    				<!-- 행 생성, 1열의 값만 헤더-->
            <th scope="row">Khiresh Odo</th>
            <td>7</td>
            <td>7,223</td>
        </tr>
    </table>
    ```

---

# 대화형

HTML은 상호작용 가능한 사용자 인터페이스  
객체를 만들 때 사용할 수 있는 요소를 지원

전역 속성을 사용한다.

## &lt;dialog&gt;

- 대화 상자 요소 [MDN문서](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)

    ```html
    전역 속성중 tabindex 는 사용할 수 없다
    open 속성이 있으면 대화상자가 화면에 보여집니다
    ```

## &lt;details&gt;

- "open" 상태일 때만 내부 정보를 보여주는 정보 공개 위젯을 생성합니다. 
요약이나 레이블은 &lt;summary&gt; 를 통해 제공할 수 있습니다.

    ```html
    <!-- 노션의 토글과 같은 기능 -->
    <details open>
      <summary>제목</summary>
      <p>details에 open이 있으면 열려있는 상태</p>
    </details>
    ```

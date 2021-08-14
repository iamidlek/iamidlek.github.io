---
title: HTML Form
author: YHole
date: 2021-07-30 +0900
categories: [Front end, html]
tags: [html, semantic, form, element, tag]
---

# 양식

입력 기능을 제공, 사용자가 내용을 채우고,  
웹사이트나 어플리케이션에 제출할 수 있다
전역 속성을 사용한다.

## &lt;form&gt;

- &lt;form&gt;은 다른&lt;form&gt;을 자식으로 포함할 수 없다

    ```
    action        : 전송한 정보를 처리할 웹페이지의 URL
    autocomplete  : 사용자의 이전 기록으로 자동 완성을 할지 결정 on(default), off
    method        : 서버로 전송할 HTTP방식 GET(default), POST
    name          : 고유한 폼의 이름
    target        : 서버로 전송 후 응답받을 방식 _self(default), _blank
    novlidate       서버로 전송시 양식 데이터의 유효성을 검사하지 않도록 지정
    ```

## &lt;input /&gt;

- 사용자에게 입력 받을 데이터 양식

    ```
    autocomplete  : 사용자의 이전 기록으로 자동 완성을 할지 결정 on(default), off 
    autofocus       페이지가 로드될 때 자동으로 포커스(문서 내 고유해야 함) 
    disabled        양식을 비활성화 
    form          : 해당 <form>의 후손이 아닐경우만, 값으로 <form>의 id 값을 넣는다 
    list	        : 참조할 <datalist>의 id 속성 값 
    type	        : 입력 받을 데이터의 종류 text(default) 
    max, min      : type 속성 값이 number일 경우만 max값 > min값 
    maxlength     : 입력 최대 길이 type 속성 값이 text, email, password, tel, url일 경우만 
    multiple        둘 이상의 값을 입력 type 속성 값이 email, file일 경우만 
    name	        : 양식의 이름 
    placeholder	: 사용자가 입력할 값의 힌트  
                    type 속성 값이 text, search, tel, url, email일 경우만
    readonly	  읽기 전용(수정 불가) 
    step	        : 유효한 증감 숫자의 간격 1(default) type 속성 값이 number, range일 경우만 
    src	        : 이미지의 URL	type 속성 값이 image일 경우만 
    alt	        : 이미지의 대체 텍스트 type 속성 값이 image일 경우만 
    value	        : 양식의 초기 값 
    checked         양식이 선택되었음을 표시 type 속성 값이 radio, checkbox일경우만
    ```

- type 속성의 값 (데이터 종류의 값)

    ```
    button     일반 버튼<button>처럼 사용
    checkbox   체크박스	
    color      색상	IE 지원 불가
    email      이메일	
    file       파일	
    hidden     보이지 않지만 전송할 양식	value 속성으로 값을 지정
    image      이미지 제출 버튼	<img />처럼 사용
    number     숫자	
    password   비밀번호 가려지는 양식
    radio      라디오 버튼	같은 name 속성 그룹 내 하나만 선택 가능
    range      범위 컨트롤	min, max, step, value(기본값) 속성 사용
    reset      초기화	해당 <form> 범위 내 모든 양식
    search     검색	
    submit     제출 버튼	해당 <form> 범위 내 고유한 양식
    tel        전화번호	
    text       일반 텍스트	
    url        절대 URL
    ```

## &lt;label&gt;

- 라벨 가능 요소(labelable)의 제목(Caption)

    ```html
    <button>, <input>, <progress>, <select>, <textarea>

    for 속성의 값으로 → 참조할 요소의 id 값

    참조
    <input type="checkbox" id="user-agreement" />
    <label for="user-agreement">동의하십니까?</label>

    포함
    <label>흐르는 문장<input type="checkbox" />한줄 작성</label>
    ```

## &lt;button&gt;

- 선택 가능한 버튼을 지정

    ```
    name        : form 데이터와 함께 전송되는 버튼의 이름
    type        : 버튼 타입 button, reset, submit
    form        : 해당 <form>의 후손이 아닐경우만, 값으로 <form>의 id 값을 넣는다
    autofocus     페이지가 로드될 때 자동으로 포커스
    disabled      버튼을 비활성화
    ```

## &lt;textarea&gt;

- 여러 줄의 일반 텍스트 양식

    ```
    name          : 양식의 이름
    form          : 해당 <form>의 후손이 아닐경우만, 값으로 <form>의 id 값을 넣는다
    autofocus       페이지가 로드될 때 자동으로 포커스
    disabled        버튼을 비활성화
    readonly        읽기 전용 (수정 불가)
    autocomplete  : 사용자의 이전 기록으로 자동 완성을 할지 결정 on(default), off
    maxlength     : 입력 가능한 최대 문자 수
    rows          : 양식의 줄 수
    place holder  : 사용자가 입력할 값의 힌트
    ```

## &lt;fieldset&gt; - &lt;legend&gt;

- 같은 목적의 양식을 그룹화&lt;fieldset&gt; 하여 제목&lt;legend&gt;을 지정

    ```
    <fieldset>
    disabled    그룹 내 모든 양식 요소를 비활성화
    form      : 그룹이 속할 하나 이상의 <form>의 id 속성값
    name      : 그룹 이름
    웹 양식의 여러 컨트롤과 <label>을 묶을 때 사용
    ```
    ```html
    <fieldset>
        <legend>Choose your favorite monster</legend>

        <input type="radio" id="kraken" name="monster">
        <label for="kraken">Kraken</label><br/>

        <input type="radio" id="sasquatch" name="monster">
        <label for="sasquatch">Sasquatch</label><br/>

        <input type="radio" id="mothman" name="monster">
        <label for="mothman">Mothman</label>
    </fieldset>
    --------------------------------------------------------

    <form action="/action_page.php">
     <fieldset>
      <legend>Personalia:</legend>
      <label for="fname">First name:</label>
      <input type="text" id="fname" name="fname"><br><br>
      <label for="lname">Last name:</label>
      <input type="text" id="lname" name="lname"><br><br>
      <label for="email">Email:</label>
      <input type="email" id="email" name="email"><br><br>
      <label for="birthday">Birthday:</label>
      <input type="date" id="birthday" name="birthday"><br><br>
      <input type="submit" value="Submit">
     </fieldset>
    </form>
    ```

## &lt;select&gt; - &lt;datalist&gt; - &lt;optgroup&gt; - &lt;option&gt;

- &lt;option&gt;, &lt;optgroup&gt;의 선택 메뉴&lt;select&gt;나 자동완성&lt;datalist&gt;을 제공
    - 각 태그별 속성

        ```html
        <select> → 옵션을 선택하는 메뉴

        autocomplete  : 사용자의 이전 기록으로 자동 완성을 할지 결정 on(default), off
        disabled        선택 메뉴를 비활성화
        multiple        다중 선택 여부
        name          : 선택메뉴의 이름
        form          : 선택메뉴가 속할 하나 이상의 <form>의 id 속성값
        size          : 행의 수

        <optgroup> → <option>을 그룹화

        label    :  필수 옵션 그룹의 이름
        disabled    옵션 그룹 비활성화

        <option> → 선택적 빈 태그로 사용 가능

        label    :  표시될 옵션의 제목 (생략시 콘텐츠 텍스트를 표시)
        value    :  양식으로 제출될 값 (생략시 콘텐츠 텍스트 값으로 사용)
        selected    옵션이 선택되었음을 표시
        disabled    옵션 그룹 비활성화
        ```

    ```html
    <select>
      <optgroup label="Coffee">
        <option>Americano</option>
        <option>Caffe Mocha</option>
    		<!-- 속성으로 값을 줄 수도 있다 -->
    	  <option label="Cappuccino" value="Cappuccino"></option>
      </optgroup>
      <optgroup label="Latte" disabled>
        <option>Caffe Latte</option>
        <option>Vanilla Latte</option>
      </optgroup>
      <optgroup label="Smoothie">
        <option>Plain</option>
        <option>Strawberry</option>
        <option>Banana</option>
        <option>Mango</option>
      </optgroup>
    </select>
    --------------------------------------------------------
    <input type="text" list="fruits">

    <!-- 자동완성기능을 위한 리스트를 제공 -->
    <datalist id="**fruits**">
      <option>Apple</option>
      <option>Orange</option>
      <option>Banana</option>
      <option>Mango</option>
      <option>Fineapple</option>
    </datalist>
    ```

## &lt;progress&gt;

- 작업의 완료 진행률을 표시

    ```html
    max     : 작업의 총량
    value   : 작업의 진행량

    텍스트 컨텐츠는 보여지지 않는다 (접근성)
    <progress value="70" max="100">70 %</progress>
    ```

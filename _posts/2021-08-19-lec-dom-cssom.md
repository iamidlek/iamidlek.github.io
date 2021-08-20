---
title: lec-[html/css] dom, cssom
author: YHole
date: 2021-08-19 +0900
categories: [Front end, lecture]
tags: [html, css, way of thinking]
---

## 기초 지식

> web : 스크린 미디어와 각종 사물을연결시키는 매개체  
> web의 진정한 힘은 그 보편성에 있다  
> 웹의 탄생은 연결이다 -팀버너스리
> html로 쉽게 프로그래밍 → http 약속을 통해 요청

> 브라우저  
> (CPU로 처리)html,css, 이미지파일등을 랜더링해서 보여준다

WHATWG(Web Hypertext Application Technology Working Group, WHATWG)
표준 약속 문서 정리


- htmlx → html5 
  - 소프트웨어지향은 그대로
  - 엄격하진 않지만 닫기 태그 잘 닫을 것
  - 소문자로만 작성할 것


- 사람의 욕망?
  - 관계, 연결, 무소부재(Ubiquitous)

- 실현시켜주는 언어

> 웹 자바스크립트

> 비동기 병렬처리가 되는 언어가 대세  
> 함수로 많이 처리  
> 함수형 패러다임을 잘 지원하는 언어  
> 클라우드 비동기 처리 언어가 향후 중요

## HTML 이해

#### 메모리 이용의 이해

- 2 진수 : 컴퓨터의 on/off
- 10 진수 : 사람이 쉽게 이해하는 방법
- 16 진수 : 4비트를 쉽게 표현하는 방법

<table cellspacing="2" cellpadding="3" width="500" align="center" border="1">
<tbody align="center">
<tr><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td></tr>
<tr style="background-color:#EAE3DC;color:#2C4047;"><td>0000</td><td>0001</td><td>0010</td><td>0011</td><td>0100</td></tr>
<tr><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td></tr>
<tr style="background-color:#EAE3DC;color:#2C4047;"><td>0101</td><td>0110</td><td>0111</td><td>1000</td><td>1001</td></tr>
<tr><td>a</td><td>b</td><td>c</td><td>d</td><td>e</td></tr>
<tr style="background-color:#EAE3DC;color:#2C4047;"><td>1010</td><td>1011</td><td>1100</td><td>1101</td><td>1110</td></tr>
<tr><td>f</td></tr>
<tr style="background-color:#EAE3DC;color:#2C4047;"><td>1111</td></tr>
</tbody>
</table>
<br/>

<li>rgb 색상 표현: #<span style="color: red;"> FF</span><span style="color: green;"> FF</span><span style="color: blue;"> FF</span></li>

- 0~15 * 0~15 = 256
- 256 ^ 3 = 2 ^ 24 = 16,777,216


- utf-8
- 16진수는 문자를 매칭할때도 사용된다
  - 문자 인코딩
    - 키번호 utf-8 → 서체.ttf 순으로 명령이 진행
  - 디코딩
    - 글꼴 적용된 글씨가 화면에 나온다


#### DOM

html은 메모리 구조 설계를 편하게 해줌

- 콘솔에 이하를 입력해보자

```
window 

window 자식으로 html이 있다

클래스 생성자 함수로
프로토타입 객체로 찍어낸다
(엘리먼트의 정보들)

이러한 체계를 돔이라고 한다
html은 돔을 만드는 방식이다

부모자식 관계를 만들고
이로서 원하는 데이터 구조를
메모리 상에 만드는 디자인 설계 도구

각 요소가 가지는 내용들은
class로 찍어내는 방식
```

- 객체로 보는 예시

```
부모요소와 정보가 있다
자식요소와 정보가 있다
자손요소와 정보가 있다

부모의 주소 : 00AB1234
부모의 데이터 : 자식 주소, 본인 데이터

자식의 주소 : 00BC1234
자식의 데이터 : 자손 주소, 본인 데이터

자손의 주소 : 00CC1234
자손의 데이터 : 본인 데이터
```


## css 이해 

- html 태그 하나당 상자하나
- cssom w3c에서 관여

- computed에는 요소당 정보뭉치가 들어있다
- css는 하나의 파일로 여러 html에 적용되는게 장점이다

```
DOM을 만들고
DOM을 가져와서 CSSOM을 만든다
html을 잘 짜야 css를 잘할 수 있다
```


#### 디폴트 렌더링

```
user agent stylesheet

웹 개발자가 디폴트css를 넣어둠
```

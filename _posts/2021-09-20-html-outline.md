---
title: html-outline
author: YHole
date: 2021-09-20 +0900
categories: [Front end, html]
tags: [js, html, outline, semantic]
---

## HTML5 outline(의미 구조)

### 테그마다 다른 역할

- 그룹핑을 하지만 의미구조에 영향을 미치지 않는 태그

```html
<header> 제목, 네비, 로고, 목차, 검색폼을 그룹핑
<main> 페이지내의 가장 주요한 부분을 그룹핑 한번만 사용
<footer> 주요 내용과 관련된 링크, 저작 관련 정보 그룹핑
```


- 의미구조에 영향을 미치는 태그

```html
<section> 자신만의 <header><footer>를 가질 수 있음
<article> 독립적 내용 <header><footer>를 별도로 가질 수 있음
<aside> 부가적 링크, 인용, 광고를 그룹핑
<nav> 주요 네비게이션 그룹핑
```


> &lt;section&gt; 을 사용하면 의미 구조에 영향을 주어  
> 안에 제목을 설정해 주지 않는다면 Untitled Section이 된다

```html
<section>
  <header>
    <h1> 제목 설정이 필요
  </header>
  <div>
  <footer>
```

### 추가된 태그

```html
<mark> 강조된 텍스트 
  검색결과 내의 검색어들을 다른 텍스트들 보다 도드라저 보이게 표시

<meter min="" max="" value=""> 
  범위와 값을 표현하기 위해 사용 (다양한 속성이 있다)

<time> 
  시간 표시용 datetime="날짜 양식" 속성 사용 가능

<figure>
  본문중 참조된 컨텐츠를 나타낼 때 사용
<figcaption>
  필수는 아니지만 컨텐츠의 주소나 제목 저작자를 표시

<progress value="" max="10">
  진행률을 표시
```

### 메모

영역 나누기에 있어서 h 태그 등 좀 짜게 주는 편인데  
이유는 목차를 만들 때 너무 많은 제목 들이나 깊이가 깊어지는  
경우를 방지하기 위해서 그랬던 것 같다.

> outline을 공부했으므로 좀 더 명확하게 목차가 만들어지는  
> 과정을 이해하고 태그를 사용할 수 있게 된 것 같다

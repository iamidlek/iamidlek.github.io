---
title: HTML Semantic
author: YHole
date: 2021-07-24 +0900
categories: [Front end, html]
tags: [html, semantic]
---

# &lt;html&gt; (메인 루트)
- 문서의 범위를 설정
    -  lang  = ko, en
    - 페이지에서 주로 사용하는 언어로 지정

## &lt;head&gt; (문서 메타 데이터)

### &lt;title&gt;
- SEO(검색 엔진 최적화)를 위해선 길고 상세하게 작성

### &lt;base /&gt;
- 파일에서 한 번만 작성, 적용할 대상의 위에 작성
  - href = "URL(시작 기준이될 경로)"
  - target = "_self"(default), "_blank", "_parent", "_top"
  - target 속성이 지정되지 않은 &lt;a&gt;, &lt;area&gt;, &lt;form&gt; 에 적용

### &lt;link /&gt;
- 외부 리소스의 연결 및 현재 문서와의 관계를 명시
- 다양한 속성이 존재하므로 유의하여 사용
  -  rel  = "icon"    href  = "경로.ico"
  -  rel  = "apple-touch-icon"    href  = "경로.ico"
  -  rel  = "stylesheet"   href  = "경로.css"
  -  rel  = "stylesheet"   href  = "경로.css" media  = "screen and (max-width: 1024px)"
  -  rel  = "preload"   href  = "경로.woff2" as  = "font"   type  = "font/woff2"    crossorigin  = "anonymous"  
  -  rel  = "stylesheet"   href  = "a.css"    title  = "a" rel  = "alternate stylesheet"   href  = "b.css"    title  = "b" 


### &lt;meta /&gt;

- 검색엔진이나 브라우저에 정보 제공
    -  charset  = "UTF-8"
    -  name  = "author"    content  = "소유자"
    -  name  = "description"    content  = "사이트 설명"
    -  name  = "viewport"  
    content  = "width=device-width, 
    initial-scale=1, user-scalable=no, maximum-scale=1, minimum-scale"
    -  http-equiv  = "X-UA-Compatible"   content  = "IE=edge"
    - &lt;base&gt;, &lt;link&gt;, &lt;script&gt;, &lt;style&gt;, &lt;title&gt; 가 나타낼 수 없는 메타 정보를 나타냄

    Open Graph (외부 링크시 보여지는 내용)
    ```html
    <meta property="og:type" content="website">
    <meta property="og:site_name" content="">
    <meta property="og:title" content="">
    <meta property="og:description" content="">
    <meta property="og:image" content="">
    <meta property="og:url" content="">
    ```

    Twitter Card
    ```html
    <meta property="twitter:card" content="summary">
    <meta property="twitter:site" content="">
    <meta property="twitter:title" content="">
    <meta property="twitter:description" content="">
    <meta property="twitter:image" content="">
    <meta property="twitter:url" content="">
    ```


### &lt;style&gt;

- 문서 내에서 스타일 정보를 작성  
  link 태그와 같이 대체 스타일 지정이 가능
    -  type  = "text/css" (default)  
    요즘 웹 문서에선 포함할 이유가 없으므로 생략 가능
    -  media  = "all and (max-width: 720px)"
    -  title  = "a"


## &lt;body&gt; (구획 루트)

### 콘텐츠 구획

### &lt;header&gt;

- 소개 및 탐색에 도움을 주는 콘텐츠를 나타냄
    - 제목, 로고, 검색 폼, 작성자 이름 등의 요소 등을 포함
    - 전역 속성을 사용

### &lt;h1&gt; - &lt;h6&gt;

- 문서나 구분된 영역의 제목 또는 문서의 목차
    - 문서의 정보 계층을 구조화
    - 전역 속성을 사용

### &lt;nav&gt;

- 문서의 부분 중 현재 페이지 내, 또는 다른 페이지로의 링크를 보여주는 구획
    - Navigation, 보통 메뉴(Home, About, Contact), 목차, 색인 등을 설정
    - 전역 속성을 사용

### &lt;section&gt;

- 문서의 독립적인 영역을 설정
    - 더 적합한 의미를 가진 요소가 없을 때 사용
    - 필수는 아니지만 일반적으로 &lt;h1&gt; - &lt;h6&gt; 을 포함하여 식별
    - 전역 속성을 사용

### &lt;main&gt;

- 문서의 주요 콘텐츠를 설정
    - IE 지원 안함
    - 문서에 한번만 사용가능
    - 전역 속성을 사용

### &lt;article&gt;

- 사이트, 문서 안에서 독립적으로 구분되거나 재사용 가능한 영역을 설정
    - 게시판과 블로그 글, 매거진이나 뉴스 기사 등
    - 전역 속성을 사용

### &lt;aside&gt;

- 문서의 주요 내용과 간접적으로만 연관된 부분을 나타냄
    - 사이드바 혹은 콜아웃 박스로 표현
    - 광고나 기타 링크 등
    - 전역 속성을 사용

### &lt;footer&gt;

- 가장 가까운 구획 콘텐츠나 구획 루트의 꼬리말 부분을 나타냄
    - 작성자, 저작권 정보, 관련 문서 등의 내용이 포함
    - 전역 속성을 사용

### &lt;address&gt;

- 가까운 HTML 요소의 사람, 단체, 조직 등에 대한 연락처 정보를 나타냄
    - &lt;body&gt;, &lt;article&gt;, &lt;footer&gt; 등에서 연락처 정보를 제공
    - &lt;a&gt; 태그가 안에 들어가는 경우가 일반적
    - 전역 속성을 사용

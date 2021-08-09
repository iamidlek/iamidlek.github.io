---
title: Docusaurus 로 블로그 만들기 - 2
author: YHole
date: 2021-07-26 +0900
categories: [Blog, Docusaurus]
tags: [Docusaurus]
---

## VS code 설치하기

구글 검색으로 **VS code**를 찾아 설치한다.  
Visual Studio Code로 md 문서 작성과 여러 속성들을 고칠 수 있다.

#### VS code는 IDE 중의 하나다?
>통합 개발 환경(Integrated Development Environment, IDE)이란  
>공통된 개발자 툴을 하나의 그래픽 사용자 인터페이스(Graphical User Interface, GUI)로 결합하는  
>애플리케이션을 구축하기 위한 소프트웨어입니다.

## Docusaurus의 구조를 알아보자

IDE로 my-website (저번 포스트에서 npx로 만든 폴더)를 열어보자.  
![initdir](/assets/img/init-docusaurus.png)  
사진과 같은 구성이 되어있다면 성공

### 바로 눈에 보이는 파일

1. .gitignore
  - github에 올리지 않을 폴더, 파일, 확장자를 지정
  - 아직은 기본값으로 두었지만 변경이 필요한 시점이 있을 것 같다.
2. README.md
  - github repo에 올렸을 때 보이는 부분이다.
  - 마크다운으로 자신이 설명하고 싶은 부분을 작성
3. package.json
  - 이전 포스트에서 설명했듯 패키지를 관리하는 파일
  - 패키지를 추가했을 시 여기에 옵션 등을 함께 기재
  - 아직 이 파일도 기본값으로 두었다.
4. **docusaurus.config.js**
  - 페이지를 수정하는 핵심 파일
  - 타이틀, 주소, 메뉴 형식 등의 내용을 수정
  - [공식 문서](https://docusaurus.io/ko/docs/configuration)를 확인해보자
  - 개발 서버를 실행하여 변화를 확인하며 수정

### static, src 폴더 내부

1. /static 에는 파비콘과 이미지 파일이 들어있다.
  - 내부에 폴더를 생성하여 ico,png,svg 등의 파일을 관리
2. /src  
  2-1. /src/pages
    - 메인 페이지에 보여지는 버튼과 배경 수정이 가능한 파일  
![index](/assets/img/indexjs.png)  
![cssmodule](/assets/img/indexcss.png)  
    - 마찬가지로 개발 서버를 실행하여 변화를 확인해 보자  

    2-2. /src/css  
    - custom.css 파일이 있다.
    - 파일명처럼 전체 영역에 대한 스타일 변경이 가능하다.
    - **개발자 도구**를 사용하여 특정 태그나 클래스를 확인 후  
스타일을 적용하면 된다.  

    2-3. /src/components
    - 메인 페이지에 보이는 3개의 그림과 글을 수정할 수 있다.

### docs, blog 폴더 내부

1. docs에는 디렉터리(폴더)와 md, mdx 파일이 들어있다.
  - 본격적으로 글을 쓰는 곳이다.
  - [공식 문서](https://docusaurus.io/ko/docs/create-doc)를 확인해보자
  - &#95;category&#95;.json 은 자신이 담겨있는 디렉터리에 대한 정의를 해준다
  - mdx 문서의 tilte, sidebar_position은 해당 문서에 대한 정의를 해준다
  - 마찬가지로 개발 서버를 실행하여 변화를 확인해 보자
2. blog 내부에는 md, mdx 파일이 들어있다.
  - 블로그를 작성하는 곳이다.
  - 폴더 구조가 아닌 tags를 달아서 모아 볼 수 있다.

## 헷갈림 Point
1. 
리액트에 대한 이해가 없다 보니 어디를 고쳐야 변화가 일어나는지 잘 모르겠다.  
일단은 위에 설명한 부분만 만져도 원하는 양식의 사이트를 만들 수 있었다.
2. 
도큐사우루스로 만들어진 블로그를 들어가  
github 링크를 타고 사이트의 코드를 확인하는 것도 도움이 많이 되었다.
3. 
개발 서버를 열어서 확인해가며 변경하면 파악이 용이하다.  
**주의점**은 터미널을 잘 봐야 한다는 것이다.  
어딘가 에러 난 코드를 작성하고 다른 곳을 고치면 갱신이 안된다.  
터미널에 에러 경고가 나왔는지 아닌지를 잘 확인해야 한다.

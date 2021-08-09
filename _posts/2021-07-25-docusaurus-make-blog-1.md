---
title: Docusaurus 로 블로그 만들기 - 1
author: YHole
date: 2021-07-25 +0900
categories: [Blog, Docusaurus]
tags: [Docusaurus]
---

## 블로그 생성 도전기

구글 검색으로 **정적 사이트 생성기(SSG, Static Site Generator)**가 있다는 것을 알게 되었다.
- 지킬(Jekyll)
- 게츠비(Gatsby)
- 휴고(Hugo)

...

여러 가지 종류가 있었고 지킬로 블로그 만들기를 도전했다.  
지킬을 선택한 특별한 이유는 없었다.  
KDT 개발 블로그 특강 시간에 이름을 들어서인 것 같다. 

html/css, js에 대한 지식이 없어서일까? 첫 도전은 실패로 끝났다.

## Docusaurus를 만나다

무엇을 검색하다 발견했는지 기억이 모호하다.  
한국어로 문서 정리가 잘 되어있어서 눈이 갔다.

그리고 도큐사우루스(Docusaurus)를 이용하여 블로그 생성에 성공한다.

### 설치 방법

[공식 문서](https://docusaurus.io/ko/docs)에 정리가 정말 잘 되어있다.  
너무 정리가 잘 되어있다 보니 설치 순서에 대한 설명은 생략하겠다.  
설치 과정 중 헷갈릴 수 있는 부분을 정리해보았다.  
(위에서 언급했듯 js에 대한 지식이 전혀 없는 상태의 사람이 느끼는 어려움이다.)

### 헷갈림 Point

#### 1. Node.js 설치 (npm, npx)

[Node.js](https://nodejs.org/en/download/)가 설치되어 있어야 한다.

##### npm도 같이 설치되는데 무엇일까?
- npm(Node Package Module)
- Node.js기반의 모듈(라이브러리)을 모아둔 저장소
- 의존성(버전관리)과 패키지를 관리하는 역할

```
npm init
```
→ package.json 파일을 만들 수 있다

```
npm install 패키지명 -g
```
→ 패키지를 설치할 수 있다 
  -g 는 전역에서 사용가능하게하는 옵션이다.  
ㅤ즉 -g 가 붙지 않으면 작업하는 폴더 내에서 설치해야한다.  
ㅤcd 작업폴더

##### npx는 무엇일까?
- npm5.2부터 npm에 들어있는 도구
- Node 패키지를 실행시킨다
- 다른 Node.js 버전으로 명령실행

→ 1회성 패키지 명령 실행 등  
ㅤnpm은 관리, npx는 실행 중심적이다.

#### 2. 개발 서버 실행

터미널에서 원하는 폴더로 이동한 후 이하의 코드를 실행
```
npx @docusaurus/init@latest init my-website classic
```
실행하면 my-website 의 폴더가 생성된다.
init(초기화) 뒤에오는 정보는 [폴더명] [스타일] 이다.  

cd my-website 으로 폴더 안으로 들어가자.
이동을 하였다면 이하의 코드를 실행  
```
yarn start
```

만들어진 사이트를 로컬에서 구동해 볼 수 있다.
웹 브라우저에 아래의 주소를 입력해보자.
```
http://localhost:3000
```

<br/>
사이트가 보인다면 블로그 만들기의 반은 해결된 것이나 다름없다.  
나머지 반은 구조의 이해와 커스텀의 영역이 되겠다.

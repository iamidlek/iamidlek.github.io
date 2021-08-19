---
title: scss-basic
author: YHole
date: 2021-08-18 +0900
categories: [Front end, practical exercise]
tags: [clone, starbucks, practical]
---

## scss를 사용해보자

- CSS Preprocessor
  - css 전처리기
  - 결과물을 css로 컴파일


- sass? scss?  
  > Sass(Syntactically Awesome Style Sheets)의 3버전에서 새롭게 등장한  
  > SCSS는 CSS 구문과 완전히 호환되도록 새로운 구문을 도입해 만든  
  > Sass의 모든 기능을 지원하는 CSS의 상위집합(Superset) 입니다

- 컴파일러
  - node-sass
  - Gulp
  - Webpack
  - Parcel

### 시작하기

1. 프로젝트 폴더 만들기(노드 모듈의 폴더명은 사용하지 말 것)
2. package.json 만들기
  ```bash
  npm init -y
  ```
3. parcel-bundler 설치(개발용)
  ```bash
  npm i -D parcel-bundler
  ```
4. package.json 설정 하기
  ```json
  "scripts": {
    "dev": "parcel index.html",
    "build": "parcel bulid index.html"
  }
  ```
5. index.html 만들기
6. main.scss 만들기
7. 연결 상태 확인하기

  ```html
  <link rel="stylesheet" href="main.scss" />
  ```

## 기초 문법

### & (부모선택자 참조)

- css의 경우 

```css
.btn.active{  
}
```

- scss의 경우

```scss
.btn{
  &.active{
  }
}

.list{
  li {
    &:last-child{
    }
  }
}
```

### 단축 속성 (중첩된 속성)

```scss
.box {
  font: {
    weight: bold;
    size: 10px;
    family: sans-serif;
  };
  margin: {
    top: 10px;
    left: 20px;
  }
}
```

### 변수 사용, 유효 범위

```scss
// 변수 variables

// 유효범위
// {} 의 내부에서 선언하면 중괄호내부에서만 사용가능

$size: 100px;

.container {
  position: fixed;
  top: $size;
  .item {
    $size: 200px; // 재할당
    width: $size;
    height: $size;
    transform: translateX($size);
  }
}

// 재할당의 유효 범위?
// .item {} 내부에서만 유효
// $size의 값 자체가 바뀌지는 않는다
```

### 산술 연산


```scss
div {
  width: 20px + 20px;
  font-size: 10px * 2;
  margin: 30px / 2; // 나누기 실패
  padding: 20px % 7;
}
```

- 나누기는 실패하는 이유?

```scss
// 단축 속성의 구분으로 사용되어 버림
span {
  font: 10px / 10px serif
}
```

- 해결 방법
  - ( 계산 내용 ) => 괄호로 묶기
  - $변수 / 2 => 변수에 값을 넣어서 계산
  - 10px + 20px / 2 => 다른 연산자와 같이 사용
  - calc(100% - 200px) => calc사용(다른 단위 계산 가능)
  
> 길이와 같이 색상도 연산이 가능하다

### Mixin (재활용 가능)

```scss
// center 라는 이름의 하나의 묶음 생성
@mixin center {
  display: flex;
  justify-content: center;
  align-items: center;
}

// 사용시에는 @include 이름
.container {
  @include center;
  .item{
    @include center;
  }
}
// 재활용 가능
.box {
  @include center;
}
```

- 인수와 기본값 설정

```scss
// 인수를 설정 (인수: 기본값)
@mixin box($size: 100px, $color: tomato) {
  width: $size;
  height: $size;
  background-color: $color;
}

// 인수는 순서대로 들어감
.container {
  @include box(200px, red);
  .item { 
    // 키워드 인수 방식을 사용하여 하나의 인수만 사용
    @include box($color: green); 
    // $size는 기본값, $color 변경 값만 작성
  }
}
```

### 반복문

```scss
// 제로베이스 아님 1부터
// 보간 방법이 조금 다르다
// 보간은 선택자 위치에만 필요로 한다

@for $i from 1 through 10 {
  .box:nth-child(#{$i}) {
    width: 100px * $i;
  }
};
```

### 함수

```scss
// 반환된 결과를 활용한다
@function ratio($size, $ratio) {
  @return $size * $ratio
};

.box {
  $width: 100px;
  width: $width;
  // 함수사용
  height: ratio($width, 9/16);
  // width에 비율을 곱한 값이 적용된다
}
```

### 색상 내장 함수


```scss
.box {
  &.built-in {
    background-color: mix(red,blue); // 색상을 섞어줌
    background-color: lighten(red,10%); // 10% 밝게 
  }
}
// darken 어둡게
  // 버튼 hover 등에 활용 가능하다

// 그외 색상 내장 함수
  // saturate 채도 graysacle, invert, rgba(색상, 투명도)
```

### 데이터 종류

- scss도 데이터의 종류가 존재한다

```scss
$number: 1; //.5 1em 1px
$string: bold; // relative "url" 
$color: red; // #ffff00, rgba(0,0,0,.1)
$boolean: true; // false
$null: null;

$list: orange, royalblue, yellow;
$map: ( 
  o: orange,
  r: royalblue,
  y: yellow
);
```

### @each 사용(리스트, 맵)

```scss
// 리스트 데이터

@each $c in $list {
  .box {
    color: $c;
  }
}

// 객체 데이터

@each $key, $value in $map {
  .box-#{$key} {
    color: $value;
  }
}
```

### @content

```scss
// 추가 사항을 작성 할 수 있음

@mixin left-top {
  position: absolute;
  top: 0;
  left: 0;
  @content;
}

.box {
  width: 200px;
  height: 200px;
  @include left-top {
    bottom: 0;
    right: 0;
    margin: auto;
  }
}
```

- @mixin에 할당되는 개념은 아님(변경 없음)
  ```scss
  @mixin left-top {
    position: absolute;
    top: 0;
    left: 0;
    @content;
  }

  .box {
    width: 200px;
    height: 200px;
    @include left-top {
      bottom: 0;
      right: 0;
      margin: auto;
    &.test{
      @include left-top
      }
    }
  }
  ```

- 컴파일 결과는 이하와 같다

  ```css
  .box {
    width: 200px;
    height: 200px;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto;
  }
  .box.test {
    position: absolute;
    top: 0;
    left: 0;
  }
  ```

### 가져오기

```scss

// @import url("./sub.scss")

// scss파일에선 .scss 축약 가능
// 여러개 가져오기
@import "./sub", "./sub2";
```

### 주석

```scss
// 컴파일시 없어질 주석

/* 컴파일 후에도 남아있을 주석 */
```

---
title: Bundle
author: YHole
date: 2021-09-22 +0900
categories: [Front end, bundler]
tags: [js, bundler, bundle]
---

## Bundle

참조 관계가 있는 모듈을 하나로 묶는 것 (의존성은 유지됨)

### 효과

1. 모든 모듈을 로르하기 위해 검색하는 시간이 단축
  > 하나의 js 파일에 모듈 A, B 등등이 묶인다
2. 사용하지 않는 코드를 제거해 준다
3. 파일 크기를 줄여준다.  
  > 파일 크기가 감소, 사용자 이용에도 도움이 된다

## webpack 이해하기

### 기본 구조

모듈의 종류
1. js
2. sass
3. hbs
4. jpg,png ...

Entry : 의존 관계의 시작점
Output : 번들 파일의 정보 설정

기본 설정의 경우
src/ 에 js 파일 등을 놓고
dist에 output이 main.js로 나온다

webpack cli 설치 후

```bash
npx webpack --target=node
```

### 설정 파일

```js
const path = require('path')

module.exports = {
  entry : '시작파일경로',
  output: {
    path: path.resolve(__dirname, 'dist')
    // __dirname 에는 절대경로가 + /dist를 해주는게 path
    filename: 'bundle.js',
  },
  target: 'node'
}
```

### Mode

환경 설정 (개발의존 , 의존)
개발 모드인지 프로덕션 모드인지

### Loader

js json 이외의 다른 파일을 가져오는 경우
모듈을 가져와 처리하는 것

```js
module.exports = {
  module: {
    rules: [loader1, loder2]
  }
}
```

### Plugin

번들 과정중에 할 수 있는 일들
웹팩 패키지 내부와 외부 저장소 플러그인이 있다


```js
const A = require('해당플러그인')

plugins: [
  new A({
    template: '경로'
  })
],
mode: 'node'
```


### 템플릿 엔진
로더 
handlebars(.hbs)

.html을 .hbs로 변경

Model 데이터 정보
Template 구조

Template -- Model
${x} = word

해서 보여지면 그걸 View라고 한다

### 청크

런타임 청크

런타임에 사용되는 코드만 따로 만들어 캐싱

벤더 청크

외부 모듈은 바뀌지 않으므로 따로 만들어 캐싱

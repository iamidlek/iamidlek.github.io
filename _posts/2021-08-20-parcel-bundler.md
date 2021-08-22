---
title: parcel-bundler
author: YHole
date: 2021-08-20 +0900
categories: [Front end, bundler]
tags: [js, bundler, parcel, package, json, booststrap]
---

## parcel bundler 

- 구성 없는 단순한 자동 번들링
- 소/중형 프로젝트에 적합

### 설치와 설정

- config와 parcel 번들러 설치

```bash
npm init -y
npm i -D parcel-bundler
```

- npm run에 parcel 을 사용하기 위한 설정

```json
  "scripts": {
    "dev": "parcel index.html --open --port 1216",
    "build": "parcel build index.html"
  },
```

```bash
npm run dev
```

- reset css 추가해보기

```html
  <link rel="stylesheet" 
  href="https://cdn.jsdelivr.net/npm/
  reset-css@5.0.1/reset.min.css">
```


### 구조

- favicon.ico는 static 디렉토리에 배치하면 적용된다
- 나머지 이미지는 images 폴더에 넣어 관리

- favicon등 정적파일들이 dist에 잘 포함되어져서 반영됨

```bash
npm i -D parcel-plugin-static-files-copy
```

```json
  "devDependencies": {
    // ...
  },
  "staticFiles": {
    "staticPath": "static"
  },
```

### parcel 공식문서

- [공식 문서](https://ko.parceljs.org/)에는 여러가지 옵션 사용법이 나와있다

```bash
# 결과물 디렉토리 설정 예

parcel bulid entry.js --out-dir bulid/output
# 결과물을 dist가아닌 원하는 위치에 출력 가능

# 개발 서버 열기 설정 예
"dev": "parcel index.html --open --port 1216"

# 포트번호, 실행시 자동 브라우저 실행 등 설정 가능
```



## 추가 설정


### Vender Prefix(공급 업체 접두사)

- 브라우저 업체별로 일부 효과가 적용 안되는 경우 방지

```bash
# 한번에 2가지 패키지 설치 하기
npm i -D postcss autoprefixer
```

### .postcssrc.js 설정

- postcss 설치 후 .postcssrc.js 생성

```javascript
// ESM 에크마스크립트 모듈 = import export 됨
// nodejs에선 CommonJS 를 사용  import export 안됨

// import autoprefixer from 'autoprefixer'

// export {
//   plugins: [
//     autoprefixer
//   ]
// }

// const autoprefixer = require('autoprefixer')

// module.exports = {
//   plugins: [
//     autoprefixer
//   ]
// }

// 축약형
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}

// 에러 발생

// PostCSS plugin autoprefixer requires PostCSS 8

// postcss랑 autoprefixer가 버전이 안맞어서 생기는 문제

// npm i -D autoprefixer@9
// 해결
```

- 브라우저 리스트 설정

```json
"browserslist": [
  // 전 세계 점유율 n% 이상을 지원
    "> 1%",
    // 최근 2개 버전을 지원
    "last 2 versions"
  ]
```


### babel 설정

```bash
npm i -D @babel/core @babel/preset-env
```
- .babelrc.js 생성

```javascript
module.exports = {
  presets: ['@babel/preset-env'],
}

// 바벨도 이하의 적용이 필요하다 config 파일에
// autoprefix에서 했으므로 넘어간다
// "browserslist": [
//   "> 1%",
//   "last 2 versions"
// ]
// }
```

- 비동기 처리 에러

```javascript
// main.js
console.log('hello parcel')

async function test() {
  const promise = Promise.resolve(123)
  console.log(await promise)
}
test()

// reference err가 뜬다
// 기본설정에서 async await를 지원하지 않음
```

- 플러그인을 설치하여 해결

```bash
npm i -D @babel/plugin-transform-runtime
```

```javascript
// .babelre.js에 이하의 내용을 추가한다
//  async await 사용 가능
module.exports = {
  presets: ['@babel/preset-env'],
  plugins: [
    ['@babel/plugin-transform-runtime']
  ]
}
```


### booststrap 사용

```javascript
// scss part
// @import "../node_modules/bootstrap/scss/bootstrap";

// 다가져오면 무거움
// import bootstrap from 'bootstrap/dist/js/bootstrap.bundle'

// 사용하는 컴포넌트만 
import Dropdown from 'bootstrap/dist/dropdown'

// 초기화

// var dropdownElementList = [].slice.call(document.querySelectorAll('.dropdown-toggle'))
// var dropdownList = dropdownElementList.map(function (dropdownToggleEl) {
//   return new bootstrap.Dropdown(dropdownToggleEl)
// })

// 번들이 아니라 개별 가져오기니까 dropdown 사용
const dropdownElementList = [].slice.call(document.querySelectorAll('.dropdown-toggle'))
dropdownElementList.map(function (dropdownToggleEl) {
  return new Dropdown(dropdownToggleEl)
})

// popper js 가 없다는 오류가 뜸

// 외부 패키지가 번들에 들어있으므로 설치가 필요
// npm i @popperjs/core
```

## 헷갈림 포인트

아직 익숙하지 않은 것 같다

어떤 패키지가 필요한지, 설치 후에는 어떤 설정이 필요한지

사용해보며 익혀야 할 것 같다

---
title: Vue-시작하기
author: YHole
date: 2021-08-25 +0900
categories: [Front end, vue]
tags: [vue, framework, js]
---

## Vue

> 사용자 인터페이스를 만들기 위한 프로그레시브 프레임워크  
> 다른 단일형 프레임워크와 달리 Vue는 점진적으로 채택할 수 있도록 설계  
> 핵심 라이브러리는 뷰 레이어만 초점을 맞추어 다른 라이브러리나 기존 프로젝트와의 통합이 매우 쉽다  
> 정교한 단일 페이지 응용프로그램을 완벽하게 지원

### cli 설치

- cli를 전역 설치

```bash
npm install -g @vue/cli
```

### 간편 프로젝트 만들기

프로젝트를 만들기 위한 디렌토리로 이동

```bash
vue create 폴더명
# 자동으로 프로젝트 및 
# 예제 컴포넌트가 작성된 상태로 생성됨
cd 폴더명
```


## webpack + vue

- 템플릿으로 이용할때는 커밋이 따라오지 않게 설치

```bash
npx degit 아이디/레포지명#브렌치명 생성할폴더명
cd 생성한폴더명
```

- 일반 의존성 웹에서도 필요한 패키지  
(문법 해석용도) @next => 최신버전 

```bash
npm i vue@next
```

- 나머지 패키지도 설치

```
npm i -D vue-loader@next vue-style-loader @vue/compiler-sfc
```


### 폴더 구조 변경
  
- static, src 폴더를 만든다
  - static
    - favicon
  - src
    - main.js
    - App.vue
    - assets(이미지)
    - components(vue)


- entry 경로도 수정이 필요하다

```javascript
module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
    clean: true
  },
}
```

- module rules도 수정해 준다 

```javascript
  module: {
    rules: [
      {
        test: /\.vue$/,
        // 하나일때는 배열 생략 가능
        use: ['vue-loader']
      },
      {
        test: /\.s?css$/,
        use: [
          // 순서 중요
          'vue-style-loader',// 추가된 부분
          'style-loader', 
          'css-loader',
          'postcss-loader',
          'sass-loader' // (먼저 동작해야됨)
        ]
      },
      {
        test: /\.js$/,
        use: [
          'babel-loader'
        ]
      },
    ]
  },
```

- vue loader

```js
const { VueLoaderPlugin } = require('vue-loader')

// ... 생략
plugins: [ // main.js 와 html이 병합됨
    new HtmlPlugin({
      template: './index.html'
    }),
    new CopyPlugin({
      patterns: [ // sttic이 dist에 copy됨
        { from: 'static' }
      ]
    }),
    // 뷰로더 플러그인
    new VueLoaderPlugin()
  ],

  devServer: {
    host:'localhost'
  }
}

```

### App.vue 작성

- index.html
  - div 만들고 id 를 설정한다(id="app")

- main.js 
  ```javascript
  // import Vue from 'vue'
  // Vue.createApp(App).mount('#app')
  import { createApp } from 'vue'
  import App from './App.vue'

  // App.vue의 내용을 html의 #app 요소에게 붙여줌
  createApp(App).mount('#app')
  ```

- App.vue
  
  ```vue
  <template>
  <h1>{{ message }}</h1>
  </template>

  <script>
  export default {
    data() {
      return {
        message: 'Hello V'
      }
    }
  }
  </script>
  <!-- h1태그에 script의 변수message가 들어간 상태 -->
  ```  

- npm run dev 로 확인

### 확장자 생략 설정

- vue파일 확장자를 안붙여도 되도록 설정

- webpack.config
  
  ```javascript
  module.exports = {
    resolve: {
      // 생략할 확장자
      extensions: ['.js', '.vue']
    },
  ```

### assets 파일 가져오기 패키지

```bash
npm i -D file-loader
```

- webpack.config

  ```js
  module: {
    rules: [
      // 가져올 파일 확장자를 정규표현식으로 작성
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        use: 'file-loader'
      }
    ]
  },
  ```

  ```javascript
  module.exports = {
    resolve: {
      extensions: ['.js', '.vue'],
    
      // 경로 별칭
      alias: {
        // ~ 면 현위치+/src 인게 된다
        '~': path.resolve(__dirname, 'src'),
        'assets': path.resolve(__dirname, 'src/assets')
      }
    },
  ```

### 경로 별칭 사용하기

- vue파일에서 사용해 보기
  - components에 새 vue파일을 만든다
  
  ```vue
  <template>
    <img 
      src="~assets/파일명.png" 
      alt="대체 텍스트" />
  </template>
  ```

  - App.vue

  ```vue
  <template>
  <h1>{{ message }}</h1>
  <ImgAttach />
  </template>

  <script>
  import ImgAttach from '~/components/새로만든Vue파일명'
  export default {
    components: {
      ImgAttach
    },
    data() {
      return {
        message: 'Hello V'
      }
    }
  }
  </script>
  ```

### 실행 및 확인 하기

```
npm run dev
```

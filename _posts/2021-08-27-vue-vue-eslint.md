---
title: Vue-ESlint
author: YHole
date: 2021-08-26 +0900
categories: [Front end, vue]
tags: [vue, framework, js, eslint]
---

## ESlint

> Ecma Script Lint : js 문법 검사 도구 (화면에 에러를 보여준다)  
> 다양한 플러그인 설치가능 (확장성이 좋다)

### 설치하기

```bash
npm i -D eslint eslint-plugin-vue babel-eslint
```

### .eslintrc.js 설정

- 설정에 관한 참고 [문서](https://eslint.org/docs/rules/)

```js
module.exports = {
  env: {
    // 브라우저에서도 노드에서도 검사
    browser: true,
    node: true
  },
  extends: [
    // vue 규칙
    //'plugin:vue/vue3-essential', // lv1
    'plugin:vue/vue3-strongly-recommended', //lv2
    //'plugin:vue/vue3-recommended',// lv3
    // js 규칙 기본적으로 권장되는 js 검사를 함
    'eslint:recommended'
  ],
  parserOptions: {
    // es6를 es5도 돌아가게 변환
    parser: 'babel-eslint'
  },
  rules: {
    // 입맛에 맞는 규칙으로 변경하는 경우
  }
}
```

### 저장 시 자동 수정

1. `ctrl` + `shift` + `p`

2. settings.json 열기  
open settings(JSON)

3. OnSave 설정을 추가 기입

```json
  ,
  "editor.codeActionsOnSave":{
    "source.fixAll.eslint": true
  },
```
> vue파일에서 잘못 쓰고 저장해도 자동으로 고쳐짐

### rules

- .eslintrc.js의 rules

```js
rules: {
      // 입맛에 맞는 규칙으로 변경하는 경우

      // > 의 끝나는 줄에 따른 에러 여부
      "vue/html-closing-bracket-newline": ["error", {
        "singleline": "never",
        "multiline": "never"
      }],
      "vue/html-self-closing": ["error", {
        "html": {
          // 빈태그
          "void": "always",
          // 내용없으면 일반 태그도빈태그처럼할껀지
          "normal": "never",
          // 컴포넌트에 대해 빈태그처럼 사용 가능하게 할껀지
          "component": "always"
        },
        "svg": "always",
        "math": "always"
      }]
    }
  }
```

- 문법적 에러의 룰을 확인하는 방법
  - 에러 가난 문장에 마우스를 올리면 해당 내용으로 이동 가능

- ex) closing-bracket에 관한 에러가 날때 클릭하면  
[관련문서](https://eslint.vuejs.org/rules/html-closing-bracket-newline.html)페이지로 이동


### 템플릿

- 환경설정을 한 파일을 템플릿으로 이용
- github에 올려놓고 받을 때

  ```bash
  npx degit 아이디/레파지토리명#브렌치명 저장할폴더명

  npm install
  # 패키지 모듈은 .gitignore때문에 한번 설치해 줘야 한다 
  ```

## 템플릿 사용

### vue 파일

- App.vue 싱글파일 컴포넌트

  ```vue
  <template>
    <!-- html 부분 -->
  </template>

  <script>
    // js 부분
  export default {

  }
  </script>

  <style>
    /* css/scss 부분 */
  </style>
  ```

### 사용해보기

- App.vue

```vue
<template>
<!-- 자바스크립트에서 오는 데이터 {{}} -->
  <h1 @click="increase">
    {{ count }}
  </h1>
  <!-- 조건문 -->
  <div v-if="count > 4">
    4보다 큽니다!
  </div>
  <!-- v- 를 디렉티브라고 한다 -->
  <ul>
    <Fruit
      v-for="fruit in fruits"
      :key="fruit"
      :name="fruit">
      {{ fruit }}
    </Fruit>
  </ul>
</template>

<script>
import Fruit from '~/components/Fruit'
export default {
  components:{
    Fruit
  },
  data() {
    return {
      // 반응성: 데이터를 갱신하면 수정된 내용이 화면에 바뀌는 것
      count: 0,
      fruits: ['apple','banana','cherry']
    }
  },
  methods: {
    // 핸들러
    increase(){
      this.count += 1
    }
  }
}
</script>

<style lang="scss">
  h1 {
    font-size: 50px;
    color: royalblue;
  }
  ul {
    li {
      font-size: 40px;
    }
  }
</style>

```

- Fruit.vue

```vue
<template>
  <li>{{ name }}</li>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped lang="scss">
// scoped 를 사용하면 유효범위가 현재 컴포넌트가 됨
  h1 {
    color: red !important;
  }
</style>
```

## 느낀점

처음 블로그 관련 테마 템플릿을 보았을 때  
구조를 알 수가 없어서 힘들었는데  
vue를 공부하면서 조금씩 왜 그런 구조인지  
어떻게 구성되어 있는지가 보이는 것 같다

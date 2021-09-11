---
title: Vue-How-to-4
author: YHole
date: 2021-09-02 +0900
categories: [Front end, vue]
tags: [vue, framework, js]
---

## Vuex

> Vue.js 애플리케이션에 대한 상태 관리 패턴 + 라이브러리  
> 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역할

### vuex 설정

```bash
npm i vuex@next
```

- 컴포넌트간 데이터 활용을 할 수 있다


src/main.js

```js
import { createApp } from 'vue'
import App from './App'
import router from './routes'

// import store from './store/index.js'
import store from './store'

createApp(App)
  .use(router)
  .use(store)
  .mount('#app')
```

### store

- 데이터 집합소 생성 및 관리

src/store/index.js

```js
import { createStore } from "vuex";
import movie from './movie'
import about from './about'

export default createStore({
  modules: {
    movie,
    about
  }
});
```

- 각 저장소를 나눌 수 있다
  - movie에 대한 저장소
  - about에 대한 저장소

### 저장소의 설정

```js
export default {
  // 모듈화 된다는것을 명시적으로 표현
  namespaced: true,
  
  // 취급해야하는 데이터
  state: function () {
    return{
      movies: [],
      message: '',
      loading: false
    }
  },
  // 화살표 함수 축약형
  // state: () => ({ movies: []})

  // computed 계산된 데이터
  getters: {
    // movieIds(state) {
    //   return state.movies.map(m => m.imdbID)
    // }
    // movieIds라는 배열이 계산된 상태로 있음
  },

  // methods!
    // 변이 
    // 관리하는 `데이터` 내용 변경 가능
  mutations:{
    // 개별 갱신
    // assignMovies (state, Search) {
    //   state.movies = Search
    // },
    updateState(state, payload) {
       // ['movies','message','loading']
      }
  },

    // 직접적으로 데이터를 수정할 수 없음
    // 비동기 동작
  actions:{
    // searchMovies( { state, getters, commit }){
      // 객체 분해 해서 가져오거나 context를 매개변수로 사용해서 가져오기 가능
      // context.state
      // context.getters
      // context.commit (mutations) 활용가능
    // }

    // payload는 함수 실행시 들어오는 인자
    // async searchMovies (context, payload) {
  }
}

```

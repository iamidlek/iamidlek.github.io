---
title: Vue-How-to-5
author: YHole
date: 2021-09-03 +0900
categories: [Front end, vue]
tags: [vue, framework, js]
---

## Vuex 활용

### store

- index.js 관리

```js
import { createStore } from "vuex";
import select from './select'


export default createStore({
  modules: {
    select
  }
});
```

- select.js

```js
export default {
  namespaced: true,
  state: function () {
    return{
      // 컴포넌트에서 넘어옴
      selected: [],
      // 스토어 내부에서 삽입
      bible:[]
    }
  },
  getters: {
  },
  // state 내용 변경 가능
  mutations:{
    loadBible (state) {
      // 초기화 코드는 매우 중요
      state.bible = []
      // json 불러오기
      const endpoint = 'https://gist.githubusercontent.com/iamidlek/08a5d8759ff657352c3cc6418c635590/raw/ea99fa8105dbe8e6759c9aff54b34d5abdff801a/en_en.json';
      fetch(endpoint)
      .then(blob => blob.json())
      .then(data => state.bible.push(...data));
    },
    select(state, payload) {
      // payload 파라미터
      // 2번째 인자는 컴포넌트에서 넘어오는 정보
      console.log(payload.selected)

      // store의 state에 컴포넌트 값 입력
      state.selected = payload.selected
    },
  },
  actions:{
  }
}
```

### 컴포넌트 to store

- 컴포넌트에서 스터어로 데이터 이동

src/components/~~~~.vue

```js
methods: {
    pick(){
      setTimeout(() => {
        this.$router.push('/search/picksearch')
      }, 1000);
      this.$refs.form.classList.add('pick')

      // mutation을 실행시키기 위한 명령
      // this.$store.commit('스토어/함수명',payload)
      this.$store.commit('select/select', {
        // selected라는 이름으로
        // 이 컴포넌트의 selected 값을 넘김
        selected: [...this.selected]
      })
    },
    all(e){
      if (e.target.value === 'All Old Testament') {
        e.target.checked ? this.selected = ['All Old Testament',...this.olds] : this.selected = []
      } else {
        e.target.checked ? this.selected = ['All New Testament',...this.news] : this.selected = []
      }
    }
```

### store to 컴포넌트

- 반대로 store의 state를 사용하는 방법


- ex) src/routes/AbCd.vue

```js
methods: {
    searchMatch(){
      if (this.$refs.input.value) {
        // $store.state.select.bible
        // 스토어의 값 스토어명 키이름
        const books = this.$store.state.select.bible.filter(book => {
          const regex = new RegExp(this.$store.state.select.selected.join('|'))
          return regex.test(book.name)
          })
        
        const inputVal = new RegExp(this.$refs.input.value,'gi')
        
        // 해당 절만 가져오기
        const verses = books.filter(verse => {
          return inputVal.test(verse.content)
          })
        
        // 변환 및 붙이기
        const html = verses.map(verse => {
          const highLight = verse.content.replace(inputVal,`<span class="hl">${this.$refs.input.value}</span>`)
          return `<li><p class="infos">${verse.name} ${verse.chapter} : ${verse.verse}</p>${highLight}</li>`
        }).join('')
        this.$refs.suggestions.innerHTML = html;
      }
    },
  }
```


### 주의점

- 함수를 콜하는 방식과 데이터의 변화 등
- 관계를 잘 파악하고 구상해서 작성할 필요가 있다

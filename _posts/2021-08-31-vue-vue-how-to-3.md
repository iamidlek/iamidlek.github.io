---
title: Vue-How-to-3
author: YHole
date: 2021-08-31 +0900
categories: [Front end, vue]
tags: [vue, framework, js]
---

## Vue 문법


### App.Composition.vue

- 기존의 방식
  - count와 message를 제어하는 함수가 섞여있음
  - 해석에 있어서 자원이 소모되는 부분

```vue
<template>
<h1 @click="increase">
{{count}} / {{doubleCount}}
</h1>
<h1>
{{message}} / {{reversedMessage}}
</h1>
</template>
<script>
export default {
  data() {
    return {
      message: '',
      count: 0
    }
  },
  computed: {
    doubleCount(){
      return this.count * 2
    },
    reversedMessage(){
      return this.message.split('').reverse().join('')
    }
  },
  methods: {
    increase() {
      this.count += 1
    }
  }
}
</script>
```

- App.Composition.vue
  - 더 정돈된 모양
  - 달라진점 체크

```vue
<template>
<h1 @click="increase">
{{count}} / {{doubleCount}}
</h1>
<h1>
{{message}} / {{reversedMessage}}
</h1>
</template>
<script>
// ref 반응성을 주기위함
import { ref, computed } from 'vue'
export default {
  setup() {
    const message = ref('')
    const reversedMessage = computed(() => {
      return message.value.split('').reverse().join('')
    })
    // ref를 주어 반응성 주기(객체 데이터가 된다)    
    const count = ref(0)
    const doubleCount = computed(() => count.value * 2)
    function increase() {
      count.value += 1
    }

    return {
      message,
      reversedMessage,
      count,
      doubleCount,
      increase
    }
  }
}
</script>
```


- watch 옵션도 옮겨 보자
- created mounted 와 같은 라이프 사이클도 이용 가능

```vue
<script>
// watch 추가로 가져오고
// on이 붙는다 (created 는 불필요)
import { ref, computed, watch, onMounted } from 'vue'
export default {
  setup() {
    const message = ref('')
    // 감시할 대상, 콜백 함수
    watch(message,newValue => {
      console.log(newValue) // 변화된 값 출력
    })
    const reversedMessage = computed(() => {
      return message.value.split('').reverse().join('')
    })

    // created와 동일한 효과 (자세한건 라이프 사이클 훅)
    console.log(message.value)

    const count = ref(0)
    const doubleCount = computed(() => count.value * 2)
    function increase() {
      count.value += 1
    }

    onMounted(() => {
      console.log(count.value)
    })

    return {
      message,
      reversedMessage,
      count,
      doubleCount,
      increase
    }
  }
}
</script>
```


### props, context

- App.vue

```vue
<template>
  <MyBtn
    class="cl"
    style="color: blue;"
    color="#ff0000"
    @hello="log">
    App
  </MyBtn>
</template>
<script>
import MyBtn from '~/components/MyBtn'
export default {
  components: {
    MyBtn
  },
  methods: {
    log() {
      console.log('Hello')
    }
  }
}
</script>
```

- MyBtn.vue

```vue
<template>
  <div
    // 속성 상속
    v-bind="$attrs"
    class="btn"
    @click="hello">
    <slot></slot>
  </div>
</template>
<script>
// 컴포지션api 정리 방법을 위한 import
import { onMounted } from 'vue'
export default {
  // 최상위 요소 상속 x v-bind로 상속
  inheritAttrs: false,
  props: {
    color: {
      type: String,
      default: 'gray'
    }
  },
  // @hello 부분
  emits: ['hello'],
  mounted() {
    console.log(this.color)
    console.log(this.$attrs)
  },
  methods: {
    // hello 정의
    hello() {
      this.$emit('hello')
    }
  },

  // 정리 방법 컴포지션api
  setup(props, context) {
    function hello() {
      context.emit('hello')
    }
    onMounted(() => {
      // this 사용 안함
      console.log(props.color)
      // $ 대신 context 를 사용
      console.log(context.attrs)
    })
    return {
      hello
    }
  }

}
</script>
```

## vue-router next

- vue 3버전 라우터


npm install vue-router@4

main.js
import router from './routes/index.js'
createApp(App).use(router).mount('#app')


routes/index.js 생성

Home.vue 생성
About.vue 생성

APP.vue

```vue
<template>
  <RouterView />
</template>
```

npm run dev


npm i bootstrap@next

scss파일에 import

App.vue 수정
```vue
<template>
  <RouterView />
</template>

<style scoped>
 @import "~/scss/main";
</style>
```

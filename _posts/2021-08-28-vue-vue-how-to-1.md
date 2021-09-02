---
title: Vue-How-to-1
author: YHole
date: 2021-08-28 +0900
categories: [Front end, vue]
tags: [vue, framework, js]
---

## Vue 문법

### &lt;template&gt;

- html을 작성하는 부분
- &#123;&#123; x &#125;&#125; 중괄호 두번으로 script의 데이터를 사용
- @, :, v-if 와 같은 디렉티브 사용

```vue
<template>
  <div
    class="main"
    :class="now">
    <nav>
      <ul>
        <li
          :class="'index'" 
          @click="tab">
          Index
        </li>
    ...
```

### &lt;script&gt;

- componets
  - 다른 vue 파일을 불러와서 사용
- data(getter,setter 읽어오기, 쓰기)
  - 변수를 선언하여 사용
- methods
  - 함수를 선언하여 사용
- computed(읽기 전용 getter)
  - 계산된 값을 반환하는 함수를 사용
  - get(), set(x)로 읽기, 쓰기 가능


```vue
<script>
import Index from '~/components/Index'

export default {
  components:{
    Index
  },
  data(){
    return {
      x: 'index'
    }
  },
  computed: {
    cal: {
      get() {
        return this.x.split('').reverse().join('')
      },
      set(v) {
        x += v
      }
    }
  },
  methods:{
    tab(e) {
      this.x = e.target.className
      }
    },
  }
</script>
```

### &lt;style&gt;

- lang으로 preprocessor 정할 수 있음
- scoped로 해당 vue파일에만 스타일 적용 가능

```vue
<style lang="scss" scoped>
</style>
```

## Vue 활용

[템플릿 문법](https://v3.ko.vuejs.org/guide/template-syntax.html#%E1%84%87%E1%85%A9%E1%84%80%E1%85%A1%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8-interpolation)

### &#123;&#123;  &#125;&#125; (Mustaches)

- 변수의 값을 반환
- html을 반환할 경우 문자열로 인식(사용 불가)
- 메서드가 올 경우 메소드명() &#123;&#123; 메소드() &#125;&#125;
- 컴퓨 티드가 올 경우 컴퓨티드명 &#123;&#123; 컴퓨티드명 &#125;&#125;

### v-html

- html을 반환하고 싶은 경우 v-html을 사용

```vue
<span v-html="변수명"></span>
```

```vue
data() {
    return {
      변수명: '<span style="color: red">내용 내용</span>'
    }
  }
```

### v-bind

- 이중 중괄호 &#123;&#123; &#125;&#125 는 html의 속성에 사용 불가
- v-bind:속성="값"을 사용한다
- 약어 표현 :

```vue
<div v-bind:id="아이디"></div>
```

### v-on

- 뒤에 이벤트를 붙여 해당 이벤트를 감시 (이벤트 청취)
- v-on:focus="함수"
  - 해당 함수를 `이벤트 핸들러`라고한다
- 약어 표현 @

```vue
<a v-on:click="함수, 함수"> </a>

...
  methods: {
    함수 (event) {
      // 실행할 내용
      // event.target 이벤트 정보
    }
  }
...
```

### 이벤트 수식어

- 이벤트 수식어
  - event.prevntDefault()
  - 수식어로 작성하면 @click.prevent="함수" 로 작성한다
    - 해당 태그의 기본 동작을 일어나지 않게 한다
    - a의 경우 링크를 타고 넘어가는 기능이 안되게 한다
  
  - @click.once="함수"  함수 호출을 한번 하면 끝
    - .prevent.once 는 prevent를 한번만 실행
  
  - 부모 요소가 자식 요소를 감싸고 있을 때
    - 자식을 클릭하면 부모도 같이 클릭이 된다
      - 이벤트 버블링 (자식 실행 후 부모 실행)
    - 자식에게 event.stopPropagation()으로 버블링 막기
    - 자식에게 @click.stop="함수" 로 버블링 막기

  - @click.capture="함수"
    - 버블링의 반대 캡쳐링
    - 자식 요소를 클릭해도 부모 실행 후 자식 실행
  - @click.capture.stop="함수"
    - 자식 요소를 클릭해도 부모 요소만 실행됨

  - @click.self="함수"
    - 부모 요소에 줄 경우 자식에 의해 가려진 부분은 제외됨
    - 정확히 부모 요소를 선택해야 부모요소 함수가 실행

  ```vue
  <template>
    <div @click="A">
      <div @click="B">
      </div>
    </div>
  </template>

  <script>
  export default {
    methods: {
      A () {
        console.log('A')
      },
      B () {
        console.log('B')
      }
    }
  }
  </script>

  ```

  - 부모에만 @click이 있을 때
    - 자식요소를 클릭하면
      - event.target = 자식요소(실제 클릭된 요소)
      - event.currentTarget = 부모요소(event가 실행된 함수를 실행한 요소)
    - 부모요소를 클릭하면
      - 두 내용이 같아진다
      - 즉 .self는 두 내용이 같을 때만 실행되도록 한 것

  - @wheel="함수"
    - 마우스 휠을 돌렸을 때 함수 실행
    - @wheel.passive="함수" 를 하면 처리 내용과 화면 움직임이 따로 실행
      - 화면, 로직을 따로 실행하므로 버벅거림이 없다

### 키 수식어

- @keydown="함수"
  - event.key는 실제 눌린 key 값이 들어있다
  - @keydown.enter는 엔터가 눌렸을 때 함수를 실행한다
  - @keydown.ctrl.shift.a 세 가지 키를 다 눌렀을 때 실행

### v-for

- 반복문
- books라는 배열이 있다면  
요소 하나하나 book으로 꺼내서 사용
- :key="book" 으로 해당 태그가 고유하다는 것을 명시
- 두 번째 파라미터를 설정하면 index가 반환
  - v-for="(book, index) in books"
- books(배열)의 데이터가 변화되면 바뀐 내용이 화면에 적용됨
  - 반응성이라고 한다

```vue
<li 
  v-for="book in books"
  :key="book">
  {{ book }}
</li>
```

### v-if (v-show)

- true면 해당 태그가 렌더링 된다
  - v-show는 false일때 display none이 되지만 렌더링은 한다
  - v-if 가 전환 비용이 높다
- &lt;template&gt; 태그에 적용하고 싶을 때
  - 자식으로 template 태그를 하나 더 만들고 v-if를 사용한다

```vue
<a 
  v-if="book === 'funny'">
  {{ book }}
</ a>
```

### watch

- 데이터의 변경사항을 감시
- x의 내용이 메소드에 의해 변경되었다
- watch 의 x()가 감지하고 실행된다
- computed의 값도 감시 가능

- 첫 번째 매개변수를 정하면 그 값에는 변경된 내용이 들어온다


```vue
<script>
export default {
  components:{
  },
  data(){
    return {
      x: 'abc'
    }
  },
  computed: {
    
  },
  watch: {
    x(c) {
      console.log('바뀌었습니다',c) // 바뀐 내용 출력됨
    }
  },
  methods:{
    click() {
      this.x = 'def'
      }
    },
  }
</script>
```

### :class="객체"

- isView의 true/false에 따라 view 클래스 적용이 결정된다
- 여러 클래스를 줄 수 있다
- 배열로도 줄 수 있다

```vue
<div :class="{ view : isView }">
</div>
<div :class="{ view : isView, 'cur-status' : isActive }">
</div>
```

- 변수에 객체를 넣어서 사용 가능

```vue
<div class="classObj">
</div>

...
data() {
  return {
    classObj: {
      view : isView,
      'cur-status' : isActive
    }
  }
}
...
```

### :style=""

- 인라인 스타일로 스타일을 줄 수 있다
- 바인딩 하여 정의된 스타일을 줄 수 있다
- 배열로 두 개 이상의 객체를 동시에 사용할 수 있다

```vue
<div :style="[fontStyle, backgroundStyle]">
</div>

...
data() {
  return {
    fontStyle: {
      color: '#333333',
      fontSize: '16px'
    },
    backgroundStyle: {
      backgroundColor: '#000000'
    }
  }
}
...
```

## 느낀점

새로운 방식들이 많지만 편리한 기능인 것 같다
하나의 vue 파일에서 모든 부분을 손볼 수 있어서 좋은 것 같다

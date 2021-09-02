---
title: Vue-How-to-2
author: YHole
date: 2021-08-29 +0900
categories: [Front end, vue]
tags: [vue, framework, js]
---

## Vue 문법

### 폼 입력 바인딩

- 단방향 데이터 바인딩
  - script에서 template으로만 정보가 간다

```vue
<template>
  <h1>{{ msg }}</h1>
  <input
    type="text"
    :value="msg" />
</template>

<script>
export default {
  data() {
    return {
      msg: 'x y z'
    }
  },
}
</script>
```

- 양방향 데이터 바인딩
  - 입력한 데이터가 msg에도 반영이 된다
  - @input="msg = $event.target.value"
  - @change="msg = $event.target.value" 의 경우
    - 엔터, 탭, 포커스 해제시 반영이 된다

- v-model
  - :value 와  @input을 한 번에 합쳐서 사용하는 의미
  - 주의 사항
    - 한글의 경우 하나의 글자가 완성되기 전까지는 반영이 안됨
    - @input 을 사용하면 해결

  - 수식어
    - v-model.lazy="msg"
      - @chage 와 같은 효과
    - v-model.number="msg"
      - 데이터 타입이 문자열이 아닌 숫자로 유지됨
    - v-model.trim="msg"
      - 앞쪽, 뒤쪽의 띄어쓰기를 없애줌
    

```vue
<template>
  <h1>{{ msg }}</h1>
  <input
    type="text"
      // :value="msg" 
      // @input="msg = $event.target.value"
    v-model="msg" />

  <h1>{{ checked }}</h1>
  <input 
    type="checkbox" 
    v-model="checked" />
</template>

<script>
export default {
  data() {
    return {
      msg: 'x y z',
      checked: false
    }
  },
  // methods: {
  //   handler(e) {
  //     this.msg = e.target.value
  //   }
  // }
}
</script>
```


### 컴포넌트

부모 자식 간의 데이터 통신

- props로 color 라는 속성처럼 값을 받는 기능을 만듬
- App.vue 에서 color의 값을 주면
- Index.vue에서 설정한 backgroundColor의 color가 지정한 값으로 적용

- props로 text를 만들어서 내용을 주는 것보다  
버튼이므로 닫히는 태그를 만들어 준다 slot 태그를 준다

App.vue

```vue
<template>
    <Index color="royalblue" />
    <Index :color="color" /> //data 값
    <Index large color="royalblue"/>
    <Index text="banana" />
    <Index>banana</Index>

</template>

<script>
import Index from '~/components/Index'
export default {
  components:{
    Index,
  },
  data() {
    return {
      color: '#ccc'
    }
  }
}
</script>
```

Index.vue

```vue
<template>
    <div
      :class="{ large }"
      :style="{ backgroundColor: color }"
      >
      // {{ text }}
      <slot><slot>
    </div>
</template>

<script>
export default {
  components:{
    Index,
  },
  props: {
    color: {
      type: String,
      default: 'gray'
    },
    large: {
      type: Boolean,
      default: false
    },
    text: {
      type: String,
      default: ''
    }
  }
}
}
</script>
```

### 속성 상속

- 최상위 요소
  - template의 자식요소가 최상위 요소
  - 하나의 최상위 요소의 경우 컴포넌트에 상속이 일어난다
    - App.vue 에서 컴포넌트 태그에 설정한 내용이  
    index.vue의 최상위 요소에 적용
  - 두 개 이상이면 어떤 태그에 상속할지 모르기 때문에  
  상속이 일어나지 않는다

- 최상위 요소 하나면서 속성 상속 안 받으려면

index.vue

```vue
<script>
export default {
  inheritAttrs: false,
  // 컴포넌트 생성시 한번 실행
  created() {
    console.log(this.$attrs)
    // App.vue proxy 정보가 넘어온다
  }
}
</script>
```

- App.vue의 설정 내용을 확인 할 수 있다

index.vue

```vue
<template>
<h1
  :class="$attrs.class"
  :style="$attrs.style">
</h1>
</template>
```

축약형 (v-bind: 가 아니다)

  - 속성과 속성 값을 가져 올 수있다

```vue
<template>
<div class="btn">
  <slot></slot>
</div>
<h1 v-bind="$attrs"></h1>
</template>

<script>
export default {
  inheritAttrs: false,
  // 컴포넌트 생성시 한번 실행
  created() {
    console.log(this.$attrs)
    // App.vue proxy 정보가 넘어온다
  }
}
</script>
```

### Emit

- App.vue에서 정의한 @anything="log"를  
index.vue에서 사용하기

- 이벤트를 상속 받음
- 컴포넌트에 상속할 때는  
꼭 이벤트 이름이 아니어도 괜찮다
- 연결 용도이기 때문
- 실제 사용하는 vue 파일에서 이벤트만 잘 정의하면 됨

- $event는 반대로 app.vue의 실행 함수에 이벤트 내용을 전달함

```vue
<template>
  <div>
    <slot></slot>
  </div>
  <h1 @click="$emit('anything', $event)">
    ABC
  </h1>
  <input 
  type="text" 
  v-model="msg" />
</template>
<script>
export default {
  emits: [
    'anything',
    'changeMsg'
  ],
  data(){
    return {
      msg: ''
    }
  },
  watch: {
    msg() {
      this.$emit('changeMsg', this.msg)
    }
  }
}
</script>
```

App.vue

```vue
<template>
  <Index
    @anything="log"
    @change-msg="logMsg">
    banana
  </Index>
</template>
<script>
import Index from '~/components/Index'
export default {
  components: {
    Index
  },
  methods: {
    log(event){
      console.log('clicked!')
      console.log(event)
    },
    logMsg(msg){
      console.log(msg)
    }
  }
}
</script>
```

### slot

slot 내부의 값은 대체 문자열이 있으면 대체된다

- named slot
  - slot name="a"
  - slot name="b" 로 순서를 정해줄수있다 
  - App.vue 에서는
    - 컴포넌트 태그사이에 v-slot 을 주어 작성한다
    - &lt;tamplate v-slot:a&gt;  
      &lt;span&gt;내용&lt;span&gt;  
      &lt;tamplate&gt;
  - 그러면 index.vue에서 지정한 name의 순서대로 나타난다

  - v-slot: 약어 #

  
### provide inject

> app => parent => child 로 데이터 연결시  
> props는 parent, child에 정의를 해주어야함  
> 중복, 복잡함을 해소하기위해 provide/inject를 사용

- provide/inject
  - props로는 parent 가서 child 가지만
  - provide/inject로 한 번에 child로 가져갈 수 있음

- 주의
  - 반응성은 유지가 안됨
  - ex) App에서 내용이 바뀌어도 child에 적용 안됨

App.vue

```vue
<script>
import parent from '~/component/parent'
export default {
  componets: {

  },
  data() {
    return {
      message: 'Hello world'
    }
  },
  provide() {
    return {
      msg: this.message
    }
  }
}

</script>
```

child.vue

```vue
<script>
export default {
  inject: ['msg']
}
</script>
```


- 반응성을 위지시키는 방법

```vue
<script>
import parent from '~/component/parent'
import { computed } from 'vue'
export default {
  componets: {
    Parent
  },
  data() {
    return {
      message: 'Hello world'
    }
  },
  provide() {
    return {
      msg: computed(() =>  this.message)
    }
  }  
}
</script>
```
child.vue   
- 원하는 결과를 얻으려면 msg.value
- 반응형이 가능해진다

```vue
<template>
  <div>
    {{ msg.value }}
  </div>
</template>
<script>
export default {
  inject: ['msg']
}
</script>
```

- 조상에서 자식
  - props
- 조상에서 후손
  - provide, inject

### refs

> 기존 js 방식의 querySelect 방법

```vue
<template>
  <h1>
    Test vue
  </h1>
</template>
<script>
export default {
  mounted() {
    const h1EL = document.querySelector('h1')
    console.log(h1EL.textContent)
  }
}
</script>
```

- ref (참조) 이용
  - this.$refs.a는 a를 ref로 가지고 있는 요소를 반환

```vue
<template>
  <h1 ref="a">
    Test vue
  </h1>
</template>
<script>
export default {
  created() {
    // 이 라이프 사이클에선 undefined가 된다
    console.log(this.$refs.a.textContent)
  }
  mounted() {
    console.log(this.$refs.a.textContent)
  }
}
</script>
```

- 컴포넌트를 import 한다

```vue
<template>
  <h1>test</h1>
</template>
```


```vue
<template>
  <Test ref="a" />
</template>
<script>
import Test from '~/component/Test'
export default {
  components: {
    Test
  },
  created() {
    // 이 라이프 사이클에선 undefined가 된다
    console.log(this.$refs.a)
  },
  mounted() {
    // 요소의 proxy가 반환된다
    console.log(this.$refs.a.$el)
    // <h1>test</h1> 가 출력됨
  }
}
</script>
```


- 컴포넌트에 요소가 2개 이상이면?

```vue
<template>
  <h1>test</h1>
  <h1 ref="other">another</h1>
</template>
```

```vue
<template>
  <Test ref="a" />
</template>
<script>
import Test from '~/component/Test'
export default {
  components: {
    Test
  },
  mounted() {
    console.log(this.$refs.a.$refs.other)
    // 해당 other를 가진 요소가 출력됨
  }
}
</script>
```

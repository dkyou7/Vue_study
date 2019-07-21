## 25. ES6 강의 소개 및 목표 소개

- es6 for Vue.js
- 최신의 자바스크립트 문법이다.
- babel 이란 사이트이다. 손이 덜 가게 하는 싸이트 공부
- 시간을 효율적으로, 빠른 재개발, 빠른 프로토타입.
- const & let, Arrow Function, Enhanced Object Literals, Modules 학습

## 26. ES6 배경과 babel 소개

- ES6란?
  - ES5 이후 2015 업데이트
  - 리엑트, 앵귤러, 뷰에서 권고하는 언어 형식
  - 문법이 간결해지면서 코딩이 쉬워짐.

- Babel
  - 브라우져별 지원 안하는 경우가 있으므로 조심하자.. transpiling 필요하다.
  - 호환 가능한 ES5로 변환시켜주는 역할 수행한다.

## 27. const & let 소개

- 블록단위`{ } ` 로 범위가 제한되었음.
- `const` : 한번 선언한 값에 대해서 변경할 수 없음 (상수 개념)
- `let` : 한번 선언한 값에 대해 다시 선언할 수 없음

```js
const s = 10;
s = 20;
//error

var a = 10;
a = 20;
// not error
```

```js
//error
let a = 10;
let a = 20;

// not error
a=30;
```

- let 은 값을 **재할당은 할 수 있고**, const는 재할당조차 못한다.

## 28. [ES5의 주요 특징] 변수 스코프와 호이스팅

- 기존 자바스크립트의 두가지 특징
  - 블록 관계없이 스코프가 설정 된다. - 변수의 Scope

```js
var sum=0;
for(var i = 1;i<=5;i++){
	sum = sum+i;
}
console.log(sum); // 15
console.log(i);	//6
```

- 블록을 벗어나도 자바슼크립트는 접근이 된다. 전역변수에 잡히기 때문이다.
  - 호이스팅(Hoisting)
    - 끌어올려진다. 바닥에서 천장까지 끌어올려진다.
    - 함수나 변수가 가장 상단에 위치한 것처럼 작용한다.
    - js 해석기는 코드 라인 순서 관계 없이 함수 선언식과 변수를 위한 메모리 공간을 먼저 확보한다.

```js
function a(){
	return 10;
}
a();	// 5
function a(){
	return 5;
}
```

- 코드 순서 재조정 어떻게 될까요?

```js
var sum=5;
sum = sum+i;

function sumAllNumber(){
...
}
var i = 10;
```

- 일단 오류가 나지 않는게 신기하다.. 

```js
// 1. 함수 선언식과 변수 선언을 hoisting
var sum;
function sumAllNumber(){
...
}
var i;
// 2. 변수 대입 및 할당.
sum=5;
sum = sum+i;
i=10;
```

## 29. const와 let 추가 설명 및 정리

28번에서 배웠던 것과는 다르게 변수를 let으로 설정하면 에러가 난다.

```js
let sum=0;
for(let i = 1;i<=5;i++){
	sum = sum+i;
}
console.log(sum); // 15
console.log(i);	// error
```

`const` 한번 지정한 값 변경 불가능

```js
const a=10;
a=20; //error
```

but, 객체나 배열의 내부는 변경할 수 있다.

```js
// 객체 선언 {}
const a={};
a.num=10;
console.log(a);	// {num:10}

// 배열 선언 []
const a=[];
a.push(20);
console.log(a);	// [20]
```

```js
function f(){
	{
	let x;
        {
         const x="sneaky";
         x="foo";	// error
        }
	 x="bar"; // not error
	 let x = "inner"; //error
	}
}
```

## 30. [리팩토링] const와 let

```Vue
<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data: function() {
    return {
      todoItems: []
    }
  },
  methods: {
    addOneItem: function(todoItem) {
      // const 로 바꾸어준다.
      const obj = {completed: false, item: todoItem};
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index) {
      this.todoItems.splice(index, 1);
      localStorage.removeItem(todoItem.item);
    },
    toggleOneItem: function(todoItem, index) {
      todoItem.completed = !todoItem.completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems: function() {
      this.todoItems = [];
      localStorage.clear();
    }
  },
  created: function() {
    if (localStorage.length > 0) {
      // let으로 바꿔준다.
      for (let i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
        }
      }
    }
  },
  components: {
    TodoHeader: TodoHeader,
    TodoInput: TodoInput,
    TodoList: TodoList,
    TodoFooter: TodoFooter
  }  
}
</script>
```

## 31. 화살표 함수 소개 및 설명

- Function 함수를 사용하지 않고 => 를 사용한다.
- 콜백 함수를 간소화한다.
- <https://babeljs.io/repl/>

## 32. 향상된 객체 리터럴

- 객체의 속성을 메서드로 사용할 때 function() 을 제거하고 사용 가능하다.
- 다음과 같이 변환 사용 가능하다.

```js
hello : function(){
  console.log('hello');
}
```

```js
hello(){
  console.log('hello');
}
```

## 33. [리팩토링] 속성 메서드의 축약 특징 리팩토링

- 괄호로 show:function() ——> show()로 변경 가능하다.
- data, methods 모두 변경이 가능하다.

## 34. 속성 명의 축약 한가지 더

```js
var a = 10;
var dic={
  //a:a;
  a
}
```

## 35. [리팩토링] 속성 메서드의 축약 특징 리팩토링 

- 받아온 컴포넌트와 속성이 같으면 하나로 축약하자.
- `Modal.vue`

```vue
<script>
import Modal from './common/Modal.vue'

export default {
  data() {
    return {
      newTodoItem: '',
      showModal: false
    }
  },
  methods: {
    addTodo() {
      if (this.newTodoItem !== '') {
        const item = this.newTodoItem.trim();
        this.$emit('addItem', item);
        this.clearInput();
      } else {
        this.showModal = !this.showModal;
      }
    },
    clearInput() {
      this.newTodoItem = '';
    }
  },
  components: {
    //Modal: Modal <-- 같은 부분을 하나로 합쳤다.
    Modal
  }
}
</script>
```

## 36. Modules - 자바스크립트 모듈화 방법

- 모듈화 이유 : 재사용 많이 하는거 필요할 때마다 가져다 쓸 수 있도록 만들어준다.
- `export default`: 인캡슐레이션, 모듈화 기능

## 37. Vuex 소개 - 상태 관리 라이브러리

- 개요 
  - 복잡한 어플리케이션의 컴포넌트를 효율적으로 관리하는 라이브러리, 혹은 상태 패턴
  - Vuex 전에 리엑트의 Flux 패턴. 뷰에서 컴포넌트를 관리하기에 필수적으로 필요한 과정이 무엇일까?
  - Vuex의 라이브러리의 주요 속성인 state(data), getters(computed), mutations(methods), actions(async await) 학습
  - Vuex를 더 쉽게 구현할 수 있는 helper 소개
  - Vuex로 프로젝트 구조화 방법과 모듈 구조화 방법 소개.

## 38. Flux 와 MVC 패턴 소개 및 Flux 등장 배경

- Vuex 란?
  - 무수히 많은 컴포넌트를 관리하기 위한 상태 관리 패턴 혹은 라이브러리
  - 뷰만의 상태관리 라이브러리, 리엑트의 Flux패턴에서 기인함
  - 중급 이상의 기술력을 요함.
- Flux 란?
  - MVC 패턴이 엄청 복잡하다는 것을 알게 됨. 이것을 해결하고자 함.
  - 전체적인 앱의 흐름을 추적하기 어렵다. => Flux 로 개선 — 모든 데이터 흐름이 한 방향으로만 흘러간다.![스크린샷 2019-07-20 오후 10.27.35](./img/스크린샷 2019-07-20 오후 10.27.35.png)
  - Action : 화면에서 발생하는 이벤트 혹은 사용자의 입력
  - Dispatcher : 데이터를 변경하는 방법 및 메서드
  - Model : 화면에 표시할 데이터
  - View : 사용자에게 비춰질 화면
- MVC와 Flux 비교
  - 위의 그림과 비교해보자.

    ![스크린샷 2019-07-20 오후 10.29.47](./img/스크린샷 2019-07-20 오후 10.29.47.png)


![스크린샷 2019-07-20 오후 10.31.24](./img/스크린샷 2019-07-20 오후 10.31.24.png)

- 데이터 흐름이 한방향이므로 예측가능하다
- Flux는 단방향 처리로 흐름 예측가능하다

## 39. Vuex 가 필요한 이유, 컨셉, 구조

![스크린샷 2019-07-20 오후 11.24.18](img/스크린샷 2019-07-20 오후 11.24.18.png)

- 어디서 이벤트를 보냈는지, 혹은 이벤트를 어디에서부터 받았는지 알기 어렵다...
  - 컴포넌트간 데이터 전달이 명시적이지 않다.
- Vuex 로 해결할 수 있는 문제
  - 컴포넌트간 데이터 전달 명시
  - 기존 패턴에서 발생하는 구조적 오류
  - 여러개의 컴포넌트에서 같은 데이터를 업데이트 할 시 발생하는 동기화 문제

![스크린샷 2019-07-20 오후 11.30.14](img/스크린샷 2019-07-20 오후 11.30.14.png)

- Vuex 구조
  - 컴포넌트 — 비동기 로직(Actions) — 동기 로직(Mutations) — 상태(State)

![스크린샷 2019-07-21 오전 12.04.58](img/스크린샷 2019-07-21 오전 12.04.58.png)

- [자바스크립트 비동기 처리와 콜백 함수 글 링크(클릭)](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/) [자바스크립트 Promise 쉽게 이해하기 글 링크(클릭)](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)

## 40. Vuex 설치 및 등록

```vue
npm install vuex --save
```

- src
  - store
    - store.js
- 일반적인 파일 구조라고 생각하자.

`store.js`

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(vuex);

export const store = new Vuex.Store({	// store(x) Store(o)
//
});
```

`main.js`

```js
import Vue from 'vue'
import App from './App.vue'
import {store} from './store/store.js'	// 등록

new Vue({
  el: '#app',
  // store:store, // 축약 속성
  store,
  render: h => h(App)
})

```

## 41. state 와 getters 소개

- Vuex 기술 요소
  - State : 여러 컴포넌트에 공유되는 데이터 `data`속성
  - getter : 연산된 state 값을 접근하는 속성
  - mutations : state 값을 변경하는 로직
  - Actions : 비동기 처리 로직. API call
- state
  - 여러 컴포넌트에 공유되는 데이터 속성

```js
//vue
data:{
  'message' : 'hello world!'
}
//vuex
state:{
	'message' : 'hello world!'
}
```

```html
<!-- vue -->
<p> {{message}} </p>

<!-- vuex -->
<p> {{this.$store.state.message}}</p>
```

- getters
  - state 값에 접근하는 속성이자 computed() 처럼 미리 연산된 값에 접근하는 속성

```js
//store.js
state:{
  num:10
},
getters:{
  getNumber(state){
    return state.num;
  },
  getDoubleNumber(state){
    return state.num*2;
  }
}
```

```Html
<p>
  {{this.$store.getters.getNumber}}
</p>
<p>
  {{this.$store.getters.getDoubleNumber}}
</p>
```



## 42. [리팩토링] state 속성 적용

- `app.vue`에 있던 todoItems 를 `store.js` 로 뺀다
- created 를 빼온다.

`store.js`

```Js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const storage = {
    fetch(){
        const arr = []; 
        if (localStorage.length > 0) {
            for (let i = 0; i < localStorage.length; i++) {
                if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
                    arr.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                }
            }
        }
        return arr;
    }
}

export const store = new Vuex.Store({
    state:{
        todoItems:storage.fetch()
    }
});
```

`TodoList.vue`

```Html
<template>
  <section>
    <transition-group name="list" tag="ul">
      <!-- <li v-for="(todoItem, index) in propsdata" class="shadow" v-bind:key="todoItem.item"> -->
      <li v-for="(todoItem, index) in this.$store.state.todoItems" class="shadow" v-bind:key="todoItem.item">
        <i class="checkBtn fas fa-check" v-bind:class="{checkBtnCompleted: todoItem.completed}" v-on:click="toggleComplete(todoItem, index)"></i>
        <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
          <i class="removeBtn fas fa-trash-alt"></i>
        </span>
      </li>
    </transition-group>
  </section>
</template>
```

## 43. mutations와 commit() 형식 소개

- mutations
  - State 를 변경할 수 있는 유일한 방법
  - `commit()`을 이용하여 동작시킬 수 있다

```js
//store.js
state:{num:10},
  mutations:{
    printNum:{
      return state.num;
    },
      sumNumber(state,anotherNum){
        return state.num + anotherNum;
      }
  }
```

```js
//App.vue
this.$store.commit('printNum');
this.$store.commit('sumNumber',10);
```

- 객체를 전달할 수도 있다.

```js
//store.js
state:{num:10},
  mutations:{
    modifyState(state,payload){
      return state.storeNum+=payload.num;
    }
  }
```

```Js
//App.vue
this.$store.commit('modifyState',{
  str:'pass from payload',
    num:20
});
```

## 44. [리팩토링] mutations 적용

`store.js`

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const storage = {
    fetch(){
        const arr = []; 
        if (localStorage.length > 0) {
            for (let i = 0; i < localStorage.length; i++) {
                if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
                    arr.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                }
            }
        }
        return arr;
    }
}

export const store = new Vuex.Store({
    state:{
        todoItems:storage.fetch(),
        headerTitle:'Todo it!'
    },
    mutations:{
        addOneItem(state,todoItem) {
            const obj = {completed: false, item: todoItem};
            localStorage.setItem(todoItem, JSON.stringify(obj));
            state.todoItems.push(obj);
        },
        removeOneItem(state,payload) {
            localStorage.removeItem(payload.todoItem.item);
            state.todoItems.splice(payload.index, 1);
        },
        toggleOneItem(state,payload) {
            state.todoItems[payload.index].completed = !state.todoItems[payload.index].completed;
            localStorage.removeItem(payload.todoItem.item);
            localStorage.setItem(payload.todoItem.item, JSON.stringify(payload.todoItem));
        },
        clearAllItems(state,todoItem) {
            state.todoItems = [];
            localStorage.clear();
        }
    }
});
```

`App.vue`

```vue
<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <!-- <TodoInput v-on:addItem="addOneItem"></TodoInput> -->
    <TodoInput></TodoInput>
    <!-- <TodoList v-bind:propsdata="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></TodoList> -->
    <TodoList></TodoList>
    <!-- <TodoFooter v-on:clearAll="clearAllItems"></TodoFooter> -->
    <TodoFooter></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data() {
    // return {
    //   todoItems: []
    // }
  },
  methods: {
    // addOneItem(todoItem) {
    //   const obj = {completed: false, item: todoItem};
    //   localStorage.setItem(todoItem, JSON.stringify(obj));
    //   this.todoItems.push(obj);
    // },
    // removeOneItem(todoItem, index) {
    //   this.todoItems.splice(index, 1);
    //   localStorage.removeItem(todoItem.item);
    // },
    // toggleOneItem(todoItem, index) {
    //   todoItem.completed = !todoItem.completed;
    //   localStorage.removeItem(todoItem.item);
    //   localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    // },
    // clearAllItems() {
    //   this.todoItems = [];
    //   localStorage.clear();
    // }
  },
  // created() {
  //   if (localStorage.length > 0) {
  //     for (let i = 0; i < localStorage.length; i++) {
  //       if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
  //         this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
  //       }
  //     }
  //   }
  // },
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  }  
}
</script>

<style>
...
</style>

```

`TodoHeader.vue`

```vue
<template>
  <header>
    <!-- <h1>TODO it!</h1> -->
    <h1>{{this.$store.state.headerTitle}}</h1>
  </header>
</template>

<style scoped>
...
</style>

```

`TodoInput.vue`

```Vue
<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" @keyup.enter="addTodo">
    <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"></i>
    </span>

    <Modal v-if="showModal" @close="showModal = false">
      <h3 slot="header">
        경고 
        <i class="closeModalBtn fa fa-times" 
          aria-hidden="true" 
          @click="showModal = false">
        </i>
      </h3>
      <p slot="body">할 일을 입력하세요.</p>
    </Modal>
  </div>
</template>

<script>
import Modal from './common/Modal.vue'

export default {
  data() {
    return {
      newTodoItem: '',
      showModal: false
    }
  },
  methods: {
    addTodo() {
      if (this.newTodoItem !== '') {
        this.$store.commit('addOneItem',this.newTodoItem);
        // const item = this.newTodoItem.trim();
        // this.$emit('addItem', item);
        this.clearInput();
      } else {
        this.showModal = !this.showModal;
      }
    },
    clearInput() {
      this.newTodoItem = '';
    }
  },
  components: {
    //Modal: Modal
    Modal
  }
}
</script>

<style scoped>
...
</style>

```

`TodoList.vue`

```vue
<template>
  <section>
    <transition-group name="list" tag="ul">
      <!-- <li v-for="(todoItem, index) in propsdata" class="shadow" v-bind:key="todoItem.item"> -->
      <li v-for="(todoItem, index) in this.$store.state.todoItems" class="shadow" v-bind:key="todoItem.item">
        <i class="checkBtn fas fa-check" v-bind:class="{checkBtnCompleted: todoItem.completed}" v-on:click="toggleComplete(todoItem, index)"></i>
        <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
          <i class="removeBtn fas fa-trash-alt"></i>
        </span>
      </li>
    </transition-group>
  </section>
</template>

<script>
export default {
  // props: ['propsdata'],
  methods: {
    removeTodo(todoItem, index) {
      // this.$emit('removeItem', todoItem, index);
      const obj = {todoItem,index};
      this.$store.commit('removeOneItem',obj);
    },
    toggleComplete(todoItem, index) {
      // this.$emit('toggleItem', todoItem, index); 
      this.$store.commit('toggleOneItem',{todoItem,index});
    }
  }
}
</script>

<style scoped>
...
</style>
```

`TodoFooter.vue`

```vue
<template>
  <div class="clearAllContainer">
    <span class="clearAllBtn" v-on:click="clearTodo">Clear All</span>
  </div>
</template>

<script>
export default {
  methods: {
    clearTodo() {
      // this.$emit('clearAll');
      this.$store.commit('clearAllItems');
    }
  }
}
</script>

<style scoped>
...
</style>
```

- `App.vue`가 하던 일을 `store.js`가 수행하게 되면서 코드가 많이 간결해졌다. 위치 추적을 쉽게 할 수 있게 되었다. 한 묶음으로 묶인 느낌이 들었다.

## 45. 왜 mutations 로 상태를 변경해야 하는가?

- State 는 왜 직접 상태를 변경하지 않을까?
  - 여러개의 컴포넌트에서 state를 변경하면 어느 컴포넌트에서 state에서 변경되었는지 추적하기 어렵다.
  - 특정 시점에 어떤 컴포넌트가 state를 변경했을까?? 추적하기 어렵다.
  - 명시적 상태 변환의 효과가 존재한다.

## 46. actions란?

- 비동기 처리로직을 선언하는 mutations이다.
- 데이터 요청, [promise](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/), async 와 같은 비동기 처리는 actions 에서 처리한다.

`store.js`

```Js
state:{
  counter:0
},
mutations:{
  addCounter(state){
    state.counter++;
  }
},
actions:{
  delayedAddCounter(context){
    setTimeOut(()=>context.commit('addCounter'),2000);
  }
}
```

`App.vue`

```js
methods:{
  incrementCounter(){
    this.$store.dispatch('delayedAddCounter');
  }
}
```



`store.js`

```Js
mutations:{
  setData(state,fetchData){
    state.product=fetchData;
  }
},
actions:{
  fetchProductData(context){
    return axios.get('https://domain.com/product/1')
    			.then(response => context.commit('setData',response));
  }
}
```

`App.vue`

```js
methods:{
  getProduct(){
    this.$store.dispatch('fetchProductData');
  }
}
```

## 47. 왜 비동기 로직은 actions 에서 처리해야할까?

- state 값의 처리 변경을 용이하게 하기 위해서

## 48 . helper 함수 및 ES6 Spread 연산자 소개




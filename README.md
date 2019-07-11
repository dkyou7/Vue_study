---
typora-copy-images-to: ./img
---

# Vue study

## 0. 시작하기 전에..

1. visual studio code, google chrome, node.js, vue.js dev tools 설치하기
2. [강좌 깃 허브 주소](https://github.com/joshua1988/learn-vue-js)
3. 관련 자료 클론 받아서 공부해보자..

## 1. 자 이제 시작해보자

1. 플러그 인을 설치하자

   - 사각박스 우측 하단 클릭

   - \- Vetur

     \- Night Owl

     \- Material Icon Theme

     \- Live Server

     \- ESLint

     \- Prettier

     \- Auto Close Tag

     \- Atom Keymap

   - 컴포넌트와 이벤트 많이 다룰 예정이다.



## 2. 뷰는 무엇인가?

- ![스크린샷 2019-07-10 오후 11.56.29](img/img1.png)

- ! 치고 탭 누르면 html 자동으로 만들어진다.

- Div#app 치면 \<div id="app">\</div> 만들어진다.

- ```Html
  Object.defineProperty(대상 객체, 객체의 속성,{
    // ... 정의할 내용
  })
  ```

- ```Html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <body>
      <div id="app"></div>

      <script>
          //console.log(div);
          var div = document.querySelector('#app');
          var str = 'hello world';
          div.innerHTML = str;

          str = 'hello world!!!';
          div.innerHTML = str;

      </script>
  </body>
  </html>
  ```
  이렇게 하면 값이 바뀔 때 마다 그대로 html 에 적용된다. 이걸 데이터 바인딩이라고 하고, 라이브러리가 감지하여 바로바로 적용시키는 것이 뷰의 특징이다.


  ```Html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <body>
      <div id="app"></div>

      <script>
          var div = document.querySelector('#app');
          var viewModel={};
          // 3. 즉시 실행. 코드를 보호한다.
          (function(){
              function init(){
                  Object.defineProperty(viewModel,'str',{
                      // 속성에 접근했을 때의 동작을 정의한다.
                      get:function(){
                          console.log('접근');
                      },
                      // 속성에 값을 할당했을 때의 동작을 정의한다 - 데이터 바인딩
                      set:function(newValue){
                          console.log("할당",newValue);
                          render(newValue);
                      }
                  });
              }

              // 1. 랜더 함수를 만들어서 코드 쪼개기
              function render(value){
                  div.innerHTML = value;
              }
              // 2. 호출하기
              init();
          })();
      </script>

  </body>
  </html>
  ```

  mac : option command i  개발자 도구 띄우기

- 리엑티비티 : 데이터의 변화에 따라 화면이 바로바로 바뀌는 것.

- new Vue 를 인스턴스라고 한다.




## 뷰 인스턴스

- 필수로 생성해야 하는 단위이다. 

- el 이 꼭 있어야 뷰를 실행할 수 있다. 이게 있어야 div 안쪽의 태그의 모든 것들이 유효하다고 한다.

- ```Html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <body>
      <div id="app"></div>

      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      <script>
          var vm = new Vue({
              // el이 있어야 뷰가 동작을 할 수 있다.
              el:'#app',
              data: {
                  message:'hi'
              }
          });
      </script>
      
  </body>
  </html>
  ```

- 생성자 함수 사용 설명서. 

- 인스턴스 재사용이 가능하게 된다. 쉽고 빠르고 좋다.

- ```html
  new Vue({
  	el: ,
  	template: ,
  	data: ,
  	methods: ,
  	...
  }
  ```

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <body>
      <div id="app"></div>
      <script>
          // 객체를 통째로 넣어주는 것이 가독성이 좋다.
          // 객체 표기법으로 넣어주자.
          var vm = new Vue({
              el:'#app',
              data:{
                  message:'hi'
              },
              methods:{
  
              },
              created:{
  
              }
          });
      </script>
  </body>
  </html>
  ```

## 뷰 컴포넌트 

- 거의 다 컴포넌트 기반으로 코딩하고 있다.

- 영역별로 코드로 구분해서 관리한다. 

  - 재사용성 증가
  - 코드의 반복 줄이기.

- 컴포넌트간의 관계가 생긴다.

- ![component](img/component.png)

- 컴포넌트를 등록할 수 있다.

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <body>
      <div id="app">
          <app-header></app-header>
          <app-content></app-content>
      </div>
  
      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      <script>
          Vue.component('app-header',{
              template:'<h1>Header</h1>'
          });
          Vue.component('app-content',{
              template:'<p>content</p>'
          });
          // 인스턴스를 생성하면 기본적으로 root 컴포넌트가 된다.
          new Vue({
              el: '#app'
          });
      </script>
  
  </body>
  </html>
  ```

- 
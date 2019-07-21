# Vue 기초 학습 가이드

## 0. 시작하기 전에..

- visual studio code, google chrome, node.js, vue.js dev tools 설치하기
- [강좌 깃 허브 주소](https://github.com/joshua1988/learn-vue-js)
- 관련 자료 클론 받아서 공부해보자..

- 플러그인 설치하기

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

## 1. 뷰는 무엇인가?

![스크린샷 2019-07-10 오후 11.56.29](img/img1.png)

- 리엑티비티 : 값 조작시마다 그대로 html에 적용되는 기능
  - 라이브러리에서 감지하여 바로 적용시키는 것이 특징이다

- new Vue 해준 것을 인스턴스라고 한다

## 2. 뷰 인스턴스

- 필수로 생성해주어야 하는 단위
- el이 꼭 있어야 뷰를 실행할 수 있다. 이것이 있어야 div 안쪽의 태그들이 유효하다

```vue
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

## 3. 뷰 컴포넌트

- 뷰의 핵심적인 기능이다
- 영역별로 코드를 구분하여 관리한다
  - 재사용성 증가
  - 코드 반복 사용 줄일 수 있음
- 컴포넌트간 관계 생성

## 4. 전역 컴포넌트와 지역 컴포넌트

- 전역으로 사용될 경우 : 플러그인
- 지역 컴포넌트 : 서비스 요소, 일반적 경우에 사용된다.

```vue
<body>
    <div id="app">
        <app-header></app-header>
        <app-footer></app-footer>
    </div>
    <div id="app2">
        <app-header></app-header>
        <app-footer></app-footer>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        Vue.component('app-header',{
            template:'<h1>Header</h1>'
        });
        new Vue({
            el: '#app',
            components:{
                'app-footer':{
                    template:'<footer>this is footer</footer>'
                }
            },
            methods:{
            }
        });
        new Vue({
            // 지역 컴포넌트에 footer가 등록되어있지 않기 때문에 error 가 발생한다!
            el:'#app2'
        });
    </script>

</body>
</html>
```

## 5. 컴포넌트간 통신 방식

- 앞서 말했듯 컴포넌트를 등록하면 서로 관계가 생긴다
- 뷰 컴포넌트는 각각 고유한 데이터 유효범위를 갖는다. 이로 인해 데이터 전달을 위해 프롭스와 이벤트를 배워보자

![props-events](img/props-events.png)

- 컴포넌트 통신 규칙이 필요한 이유
  - 컴포턴트 통신 데이터 통신간 방향 규칙이 없다.
  - 이를 규칙화 해주는 것이 프롭스와 이벤트이다.
  - 데이터의 흐름을 추적할 수 있다. 
  - 아래에서 위로 이벤트가 올라가고 위에서 아래로 프롭스가 내려간다.

## 6. 프롭스

- 위에서 아래로 정보가 내려갈 때 사용한다.

```vue
<body>
    <div id="app">
        <!--props 문법-->
        <!-- <app-header v-bind:프롭스 속성 이름="상위 컴포넌트의 데이터 이름"></app-header> -->
        <app-header v-bind:propsdata="message"></app-header>
        <!--2. 내려온데이터는 어떤 변수를 가져올 것인가??-->
        <app-content v-bind:propsdata="number"></app-content>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>
        var appHeader = {
            //{{이걸 데이터 바인딩이라고 한다.}}
            template:'<h1>{{ propsdata }}</h1>',
            props:['propsdata']
        }
        var appContent = {	
            template:"<h1>{{propsdata}}</h1>",
            // 1. 여기에다 변수를 먼저 등록한다.
            props:['propsdata']
        }
        new Vue({
            el:'#app',
            components:{
                'app-header':appHeader,
                'app-content':appContent
            },
            data:{
              // 2. 상위 컴포넌트의 데이터 이름.
                message:'hi',
                number:10
            }
        })
    </script>
</body>
</html>
```

## 7. 이벤트

- 아래에서 위로 정보가 올라갈 때, 버튼 클릭 이벤트 시 발생한다
- 요청을 받아 위쪽에서 데이터 처리 후 뿌려주는 방식으로 코드 흐름이 진행된다




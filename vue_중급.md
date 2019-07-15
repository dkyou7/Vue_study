# vue 중급 시작하기!

## 0 . 시작하기 전에

- visual studio code 설치하기!
  - vetur
  - tslint : 문법 오류, API 오류 해결을 위해
- Node.js, Chrome, Vue.js 크롬 플러그인, Github.com 설치하기!

## 1. Vue-cli로 프로젝트 실행하기

- 아무것도 없는 빈 폴더부터 시작한다면?

  - `bash`

    ```bash
    node -v 
    npm -v
    npm install -g @vue/cli
    ```

  - 에러가 났다면?  permission error 일 가능성이 크다. 권한이 없기 때문에 나는 에러이다.
    - 앞에 `sudo`를 붙여주자.

    `vue-cli 2.x version`
    
    ```bash
    vue init webpack-simple vue-todo
    
    cd vue-todo
    npm install
    npm run dev
    ```
    `vue-cli 3.x version`
    
    ```bash
    vue create 프로젝트 폴더 위치
    ```
    
  - 프로젝트 생성
  
    `bash` `3.x` 
  
      ```bash
      vue create vue-todo
      cd vue-cli
      npm run serve
      ```
    
    
  

## [중요] VS code에서 git bash 사용하기

- VS code 실행 후 `ctrl`+`,`입력 후 설정에 들어갑니다.
- 검색창에 `  terminal.integrated.shell.windows` 입력합니다.
- `json` 형식 나올텐데 밑에꺼 복붙하면 됩니다.
    
    ```
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
    ```



## 2. 만들 앱 설계

- todo 앱 만들 예정
  - 헤더 - TodoHeader.vue
  - 인풋 - TodoInput.vue
  - 리스트 - TodoList.vue
  - 푸터 - TodoFooter.vue

## 3. 컴포넌트 생성 및 등록하기

- `components` 폴더 -->   `TodoHeader` 파일 생성

  ![1563210857831](img/1563210857831.png)

- `App.vue` 스켈레톤 코드 작성하기

- 대박 단축키 하나 찾음 `ctrl`+`D` 하고 마우스로 같은단어 클릭하면 두개 동시에 바꿀 수 있도록 지원해준다.. 대박쓰!

- ```vue
  <template>
    <div id="app">
      <!-- 표시하기 위해 컴포넌트에 등록된 요소를 배치해주자 -->
      <todo-header></todo-header>
      <todo-input></todo-input>
      <todo-list></todo-list>
      <todo-footer></todo-footer>
    </div>
  </template>
  
  <script>
  // 1. 먼저 이렇게 임포트 해준다.
  import TodoHeader from './components/TodoHeader.vue';
  import TodoInput from './components/TodoInput.vue';
  import TodoList from './components/TodoList.vue';
  import TodoFooter from './components/TodoFooter.vue';
  
  
  export default {
    // 2. 쓰려면 컴포넌트로 등록해주자.
    components:{
      'todo-header':TodoHeader,
      'todo-input':TodoInput,
      'todo-list':TodoList,
      'todo-footer':TodoFooter
    }
  }
  </script>
  
  <style>
  
  </style>
  ```

## 4. 파비콘, 아이콘, 폰트, 반응형 태그 설정하기

`index.html`을 건들일거다.

- 파비콘 제작 : <https://www.favicon-generator.org/>

- 폰트 어썸(아이콘 제작) : <https://fontawesome.com/>
   - 유저별 키트 하나씩 발급하는데 이 키트하나로 이용가능하다.

- 구글 폰트 : [http://noonnu.cc](http://noonnu.cc/)

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>vue-intermediate</title>
    <!-- 1. 뷰포트 추가 -->
    <meta name="viewport" content="initial-scale=1, width=divice-width">
    <!-- 2. favicon 추가 -->
    <link rel="shortcut icon" href="src/assets/favicon.ico" type="image/x-icon">
    <link rel="icon" href="src/assets/favicon.ico" type="image/x-icon">
    <!-- 3. awesomefont 추가 -->
    <script src="https://kit.fontawesome.com/15453d02ad.js"></script>
    <!-- 4. google font 추가 -->
    <link href="https://fonts.googleapis.com/css?family=Ubuntu&display=swap" rel="stylesheet">

  </head>
  <body>
    <div id="app"></div>
    <script src="/dist/build.js"></script>
  </body>
</html>

```

## 5. TodoHeader 컴포넌트 구현

- 
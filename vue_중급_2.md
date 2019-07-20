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


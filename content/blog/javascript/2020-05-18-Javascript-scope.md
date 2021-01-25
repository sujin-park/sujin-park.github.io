---
layout:     post
title:      "[Javascript] scope"
date:       2020-05-18 17:50:00
author:     "sujin-park"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
multilingual: false
tags:
    - Javascript
---
scope
========================

scope란 변수의 유효 범위 또는 코드가 유효한 범위라고 할 수 있다.

컴파일러가 생성한 코드를 실행할 때, 엔진은 변수 a가 생성된 적 있는지 스코프에서 검색한다.

### 지역변수

함수내에 정의된 **지역변수**는 지역 범위를 가지며, 해당 함수와 내부 함수에서만 접근이 가능하다.
만약 같은 이름의 전역변수와 지역변수가 존재 할 경우, 지역변수가 우선순위가 높다.

### 전역변수

함수의 외부에서 선언된 모든 변수는 전역 범위(global scope)를 가진다. 브라우저에서, 전역 컨텍스트(또는 scope)는 window 객체를 가리킨다.

### 블록 스코프

블록 스코프는 **중괄호{}** 로 감싸진 범위를 말한다.

```jsx
if (true) {

}

for (let i = 0; i <= 10; i++) {

}

function func () {

}
```

### 함수 스코프

블록 스코프 중 함수 function func () 범위를 갖는 스코프를 말한다.

```jsx
function func(){
}
```

### var

var는 함수 스코프에서 유효하다. var는 중복 선언도 허용하기 때문에 한 스코프 내에서 동일한 변수명으로 선언한다면 값이 덮어씌워질 수 있다. (클로저, 호이스팅)

⇒ 함수 스코프에서 var 로 작성된 선언문이 실행된다면 함수가 종료되기 전까지 유효하다.

⇒ 내부 스코프에서 외부 스코프로는 참조가 가능하지만, 외부 스코프에서 내부 스코프로는 참조가 불가능하다.

** 전역 스코프는 함수 스코프의 내부와 동일하게 동작한다.

```jsx
function main(){
  for(var i=0; i<5; i++){
    var temp = i;
  }
  console.log(temp);
}
main();
```

### let

함수 스코프에서 var가 의도치않게 동작하는 일들을 막기 위해 es2015 에서 let과 const 가 등장하였다.

let은 블록 스코프에서 유효하다. 외부 스코프와 내부 스코프에서 같은 변수명으로 선언 되어도 다르게 할당된다.

```jsx
let name = 'Park sujin';
if(true){
  let name = 'Sujin';
	console.log(name) // Sujin
}
console.log(name); // Park sujin
```

### const

const 는 let 과 동일하게 블록 스코프에서 유효하다.

선언과 동시에 할당을 해야하며, 재할당이 불가능하다.

출처 : [https://yuddomack.tistory.com/entry/자바스크립트-스코프scope](https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8A%A4%EC%BD%94%ED%94%84scope)

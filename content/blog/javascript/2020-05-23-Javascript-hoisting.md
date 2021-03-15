---
title: '호이스팅 (Hoisting)'
date: 2020-05-23 17:17:00
category: 'javascript'
draft: false
---


호이스팅이란 변수의 선언문을 유효범위의 최상단으로 끌어올리는 행위이다.

- 함수 실행 전 해당 함수를 한 번 훑는다.

- 함수 내에 존재하는 변수/함수선언에 대한 정보를 기억 후 실행.

- 실제로 코드가 끌어올려지는 건 아니며, 자바스크립트 Parser 내부적으로 끌어올려서 처리하는 것이다.

- 실제 메모리에서는 변화가 없다.

- var로 선언된 선언문과 함수 선언문만 가능하다.

출처 : [https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)

```jsx

if(true){

var name = 'Park sujin';

}

console.log(name);

/* 호이스팅으로 변환된 코드 */

var name;

if(true){

name = 'Park sujin';

}

console.log(name);

```

var는 함수 스코프에서 유효하기 때문에, 선언문이 글로벌 스코프가 아닌 유효 스코프 최상단으로 올라온다.

변수의 선언이 초기화나 할당시에 발생하는것이 아니라, 최상위로 호이스트 된다.

### 함수 호이스팅

함수 또한 호이스팅의 대상이므로 유효한 스코프 내에서 어떤 위치에서든 함수를 선언할 수 있다.

```jsx

study();

function study () {

console.log('sujin');

}

```

### 호이스트 되었을 때 순서

1. 변수 선언

2. 함수 선언

3. 변수 할당

- 변수 선언 → 함수 선언

```jsx

var myName; // string

function myName() {

console.log("Sujin");

}

// 함수 선언은 변수 선언을 덮는다.

console.log(typeof myName); // function

```

- 함수 선언 → 변수 할당

```jsx

var myName = "Sujin";

function myName() {

console.log("Print Sujin");

}

console.log(typeof myName); //string

```

예제

```jsx

var myName = "sujin";

function myName() {

console.log("hello sujin");

}

function yourName() {

console.log("everyone");

}

var yourName = "bye";

console.log(typeof myName);

console.log(typeof yourName);

```

### 참고

function내부에서 var를 붙이지 않고, 변수를 바로 사용하는 경우 해당 변수가 function 내부의 로컬변수가 아니라, 브라우저 최상위객체(window)의 프로퍼티로 설정되며, 위의 코드를 실행하는 경우 최초에는 “undefined” 상태에서 function을 호출한 이후 전역변수(실제로는 최상위객체의 프로퍼티)로 설정되어서, 정상적으로 값을 출력한다.

[https://yuddomack.tistory.com/entry/자바스크립트-호이스팅Hoisting](https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85Hoisting)

[https://developer.mozilla.org/ko/docs/Web/JavaScript](https://developer.mozilla.org/ko/docs/Web/JavaScript)

[http://chanlee.github.io/2013/12/10/javascript-variable-scope-and-hoisting/](http://chanlee.github.io/2013/12/10/javascript-variable-scope-and-hoisting/)

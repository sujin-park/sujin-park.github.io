---
title: '타입과 값'
date: 2020-06-16 17:17:00
category: 'javascript'
draft: false
---

타입과 값
========================

이전에 자바스크립트 스코프와 호이스팅에 대해서 다시 공부를 하고 포스팅을 했다.

기본기, 내구성을 다지고자 자바스크립트 공부를 기초부터 전체적으로 공부를 하고 있는데 이번에는 그 중 가장 기초가 되는 타입과 값에 대해서 포스팅을 하려고 한다.


## 타입

`타입`이란 자바스크립트 엔진, 개발자가 어떤 값을 다른 값과 분별할 수 있게 해주는 고유한 특성이다.

### 데이터 타입

타입에는 Primitive type 과 Reference type 이 있다. 값 타입은 typeof 연산자로 알 수 있다.

**Primitive type** 
- null
- undefined
- boolean
- number
- string
- symbol ( ECMAScript6 부터 추가됨 )

**Reference type**
- Object

### 배열

자바스크립트 배열은 타입이 엄격한 언어와 달리 어떤 타입이라도 할당할 수 있다. 배열의 크기를 미리 정하지 않아도 선언이 가능하고 원하는 값을 이후에 추가하면 된다.

```javascript
let arr = ["a", 3, [1, 2, 3]];

arr.length; // 3
arr[0] === "a" // true
arr[2][0] === 1; // true
```

배열의 크기를 미리 정하지 않고 이후에 할당하는 경우에는 혼란이 있을 수 있다.

```javascript
let arr = [];

arr[0] = "a";
arr[2] = "c";

arr[1]; // undefined

arr.length; // 3
```

arr[1]의 값은 undefined 일 것 같지만, 사실은 arr[1] = undefined 로 할당해준 것과는 다르고 undeclared (변수 자체가 선언이 된 적이 없는) 라고 볼 수 있다.


### 유사 배열

`유사배열`은 key가 숫자고 length라는 속성을 가지고 있는 객체로 배열처럼 arr[0], arr[1], arr.length 처럼 활용 할 수 있다. 그러나 차이점은 **배열의 메서드를 사용 할 수 없다.**

이럴 때 배열 프로토타입에서 메서드를 빌려와서 사용을 할 수 있다.

```javascript

const todos = {
  0: "공부하기",
  1: "글 정리하기",
  2: "트위터 보기",
  length: 3
}

Array.prototype.forEach.call(todos, todo => consoel.log(todo);
```

ES6에서는 Array.from()을 사용할 수 있다.
```javascript
Array.from(todos).forEach(todo => console.log(todo));
```

### 문자열

문자열은 유사배열이지만 **불변(immutable)값**이고 배열은 **가변(mutable) 값**이다. 문자열은 불변 값으로 배열의 메서드를 빌려 쓸 수 없기때문에 reverse() 같은 메서드를 사용 할 수 없다.

그래서 문자열을 거꾸로 뒤집고 싶을 때는 문자열을 배열로 바꾸고 작업을 수행 한 후 다시 문자열로 되돌리는 방법을 사용한다.

```javascript
let text = 'todo'.split("").reverse().join("");

console.log(text); // odot
```

### 숫자

자바스크립트의 숫자 타입은 number가 유일하고 정수, 부동 소수점 숫자를 모두 포함한다.

ES6부터는 `Number.isInteger()` 로 어떤 값의 정수 여부를 확인한다.
```javascript
Number.isInteger( 50 ); // true
Number.isInteger( 50.000 ); // true
Number.isInteger( 50.3 ); // false
```

ES6 이전 버전을 위한 폴리필은 아래와 같다.
```javascript
if (!Number.isInteger) {
  Number.isInteger = (num) => {
    return typeof num === "number" && num % 1 === 0; 
  }
}
```

### 참고
[You Don’t Know JS 타입과 문법, 스코프와 클로저](http://www.yes24.com/Product/goods/43219481?scode=032&OzSrank=8)

| 카일 심슨 저 / 이일웅, 최병현 공역 | 한빛미디어 | 2017년 07월 01일

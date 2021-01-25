---
layout:     post
title:      "[우아한테크러닝] React & TypeScript 3기 - 1회차"
date:       2020-09-02 22:00:00
author:     "sujin-park"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
multilingual: false
tags:
    - React
---
우아한테크러닝 React & TypeScript 3기 - 1회차
========================

TypeScript로 Type 체크만 하는 정도로 써보고 깊게 사용해보지 못했는데,
TypeScript와 React 기반으로 컴포넌트, 아키텍쳐 설계를 함께 학습할 수 있는 기회가 생겼다.
항상 혼자보는 목적으로 기록을 해왔는데 이번엔 블로그에도 올려보자는 생각으로 좀 더 열심히 기록을 하기로 했다.


### 여러분들의 고민

- 나는 잘 하고 있나 ?
- 네트워킹 희망
- 코드 품질
- 아키텍쳐
- 적정기술
    - 프로젝트에 맞는 적정 기술을 선택하였는지 아닌지, 정답이 있는 문제인지 고민.

### 그래서, 강의에서 전달하고 싶은 것은 무엇인가?

- 도구
    - 도구의 사용법
    - TypeScript, React, Etc....
    - 상태관리자, 비동기 워크플로우, 테스팅...
- 목표, 바램, 기대
    - 상태
    - 환경
        - 다양한 실행환경을 어떻게 대응 할 것인가
    - 제품
    - 목표
    - 코드
    - 상대적


### 앞으로 사용할 도구

- [TypeScript playground](https://www.typescriptlang.org/play)
    - 예제는 [CodeSandbox.io](http://codesandbox.io) 에서 할 예정
- [React 공식 문서](https://reactjs.org/)
- [Redux](https://redux.js.org/) & [MobX](https://mobx.js.org/README.html) 상태관리
- [Redux-saga](https://redux-saga.js.org/) 와 generator, async function 도 함께 다루게 될 예정
- [Blueprint UI Framework](https://blueprintjs.com/)
    - TypeScript 기반의 UI Framework 로 TypeScript Class, Generic 등 기능을 많이 활용하면서 어떻게 React 컴포넌트화를 잘 할 수 있는지에 대해 학습하기 좋은 Framework 이다.
- [Testing Library](https://testing-library.com/)

```
요즘 오픈소스 컨트리뷰톤을 하면서 컨트리뷰션에 관심이 많은데 Blueprint UI Framework 에 기여해보고싶다!
```


### 타입스크립트 Intro.

```jsx
let foo: number = 10; // 타입 추론을 통해 foo 를 number 로 인지한다.

foo = false; // foo 가 number type 으로 되었기 때문에 type 이 다르다고 오류 발생한다.

// 가변인자를 처리하는 함수구나라는걸 알려줄 수 있다. (...args)
function bar(...args) {

}
```

- Docs 에 Basic Type 을 읽어보면 좋을 것 같다.
- 암묵적인 것보다는 명시적인 것이 보통 좋다고 생각하고, 스펙을 만드는 개발자들도 비슷한 생각을 한다.
- 난해한 코드보다는 명시적이고 읽기 쉬운 코드를 (코드가 길어진다하더라도) 더 선호한다.

```jsx
type Age = number; // type alias.

let age: Age = 10;
let weight: number = 70;
```

- type alias 는 transpiling 되면 다 사라진다. ( javascript 에서는 안보임 )
- **type alias 는 컴파일 타임에만 작동되는 문법 요소이다.**
    - compile 에만 작동되는 요소
        - Interface, Generic
    - runtime 에만 작동되는 요소
        - javascript transpiling 되는 것들

```jsx
// nesting

type Age = number;

type Foo = {
	age: Age;
	name: string;
};

const foo: Foo = {
	age: 10,
	name: 'kim',
}

// type 과 interface 둘 다 비슷한 형태로 보이지만 이후 강의에서 다룰 예정이다.

interface Bar {
	age: Age;
	name: string;
}

const bar: Bar = {
	age: 10,
	name: 'kim',
}
```

### React Intro.

```jsx
yarn create-react-app my-app --template typescript
```

tsconfig.json

- typescript compiler option 설정을 해줄 수 있다.


```jsx
// React package 를 트랜스파일링 된 후에 사용을 하기 때문에 꼭 필요하다.
import React from 'react';

function App() {
	return (
		<h1 id="header">Hello Tech</h1>
	)
}

**Babel trasnpiling**

function App() {
	return React.createElement("h1", { id: "header" }, "Hello Tech");
}
```

```jsx
import React from 'react';

// transpiling 됐을 때, 2번째 인자로 객체를 통해 값을 주기 때문에 객체로 넘어온다.
function App(props) {
	return (
		<h1>{ props.title }</h1> // transpiling 을 했을 때 props 가 넘어온 변수라는걸 체크해줄 수 없기 때문에 {} 로 구분해준다.
	)
}

```

### Redux

- Flux 아키텍쳐 기반으로 만들어진 상태관리 라이브러리
- React 자체가 전역 컴포넌트의 상태를 immutable 한 형태를 지향하기 때문에 Flux 아키텍쳐 기반이다.

### MobX

- 상태를 바라보는 관점도 다르다.
- 기능도 많고, 다양한 유형의 Practice 가 많기 때문에 어떤 형태가 더 좋은건지 혼란을 줄 수 있다.
- 유연성이 가진 장점도 있고, 단점도 있다.
- MST → MobX State Tree.


### Q & A 중 좋았던 내용
- 공부해야 할 것들은 많고, 자신이 아직 부족하다고 느끼면서 공부를 하는데 좋은 시니어가 되기 위해 주니어 때 놓치지 말아야 할 부분이 무엇이 있을지에 대해 Q & A 에 올렸었다. 강사님께서 너무 좋은 답변을 해주셔서 메모해 두었다.
    - 주니어에게는 압도적으로 많은 시간이 있기 때문에 충분한 시간과 여유를 가지고 공부를 할 수 있으면 좋겠다고 해주셨다. **공부에 방해가 되는 것은 초조한 생각**이고 **개발은 마라톤이기 때문에 천천히 충분한 시간을 가지고** 공부를 하면 좋을 것 같다고 해주셨다.


첫 시간이어서 광범위한 부분을 짧게 설명해주셔서 전체적으로 알고 있던 부분도 한번 더 되새기고, 몰랐던 부분도 알 수 있는 시간이었다. 2시간 반인데 쉬는시간에도 질의응답을 해주셔서 정말 알찬 시간이었다. 2시간 반이 이렇게 짧은 시간이었다니..
그나저나 다시 React TypeScript 강의도 듣게 되었는데 Gatsby로 블로그를 바꿔야겠다. 지금 가독성이 현저히 떨어진다..
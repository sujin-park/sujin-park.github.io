---
title: '[우아한테크러닝] React & TypeScript 3기 - 3회차'
date: 2020-09-13 17:00:00
category: 'study'
draft: false
---

3회차는 React 의 기본적인 내용과 간단하게 구현해보는 것에 중점을 둔 강의였다.

기술을 단순하게 사용만 해보는 것이 아니라 직접 구현을 해보면서 어떻게 동작 & 구현되어있는지를 보고, 기술을 습득해야한다. 단순한 사용자로만 남으면 10년 뒤에 사람이 달라진다라는 강사님의 명언으로 시작 된 3번째 강의 내용이다.



### React 만들어보기

React.js 의 사상을 이해하는 데에 문제 없는 정도의 핵심적인 부분만 만들어본다.
Real DOM API 의 추상도가 낮기 때문에 Real DOM 을 직접 조작하게 되면 복잡도가 높아진다는 것을 알 수 있다. 아래는 아주 간단한 리액트 만들기의 시작인 코드이다.

**index.js**
```jsx
const list = [
  { title: "React에 대해 알아봅시다." },
  { title: "Redux에 대해 알아봅시다." },
  { title: "TypeScript에 대해 알아봅시다." }
];

const rootElement = document.getElementById("root");
```

위 코드로 화면을 그리는 방법은 아래 코드와 같다.
```jsx
function app(items) {
  rootElement.innerHTML = `
    <ul>
      ${items.map(item => `<li>${item.title}</li>`).join("")}
    </ul>
  `;
}

app(list);
```

### React 의 컨셉

DOM에 직접 접근하여 Real DOM API 를 통해 작업하는 것은 규모가 커지면 복잡해지기 때문에 React 라이브러리는 DOM 을 Virtual DOM 으로 변환하여 Javascript 에서는 쉬운 상태를 유지하면서 작업할 수 있게 하였다.

React, Vue 같은 transpiling 이 되는 코드 같은 경우에는 컴파일 타임이라는 새로운 레이어가 생겼기 때문에 꼭 인지하고 있어야 한다.


```jsx
/* @jsx createElement */
function renderElement(node) {
  if (typeof node === "string") {
    return document.createTextNode(node);
  }

  const el = document.createElement(node.type);

  node.children.map(renderElement).forEach(element => {
    el.appendChild(element);
  });
  return el;
}
```

```/* @jsx createElement */``` 라인은 compile time에 babel이 transpiling 하여 React 대신 createElement 를 사용할 수 있다.


```jsx
function render(virtualDom, container) {
  // 여기서 virtual dom 과 새로운 virtual dom 을 비교해서 변경된 부분만 appendChild 해준다.
  container.appendChild(renderElement(virtualDom));
}

const vdom = {
  type: "ul",
  props: { item: "acac", id: "hoho" },
  children: [
    { type: "li", props: { className: "item" }, children: "React" },
    { type: "li", props: { className: "item" }, children: "Redux" },
    { type: "li", props: { className: "item" }, children: "TypeScript" }
  ]
};

function createElement(type, props = {}, ...children) {
  if (typeof type === "function") {
    return type.apply(null, [props, ...children]);
  }
  return { type, props, children };
}

function Row(props) {
  return <li>{props.label}</li>;
}

function StudyList(props) {
  return (
    
    <ul label="하하하">
      <Row label="하하하" />
      <li className="item">Redux</li>
      <li className="item">TypeScript</li>
      <li className="item">mobx</li>
    </ul>
  );
}

function App() {
  const vdom = createElement("ul", {}, createElement("li", {}, "React"));

  return (
    <div>
      <h1>1</h1>
      <StudyList item="acac" id="hoho" />
    </div>
  );
}

render(<App />, document.getElementById("root"));

```

React는 한개의 Virtual DOM 을 가지고 있고, render 함수를 호출 할 때 변경된 부분만 리렌더링을 해준다.

HTML built in component는 소문자로 시작하기 때문에 HTML 로 들어가지만, 사용자가 생성한 컴포넌트는 대문자로 선언하면 바벨에서 function 으로 변환해준다. 그래서 컴포넌트를 생성할 때 대문자로 시작하게끔 선언해준다.

```jsx
// 사용자 컴포넌트 대문자로 사용.
render(<App />, document.getElementById("root"));
```

```jsx
// babel transpiling. function App 을 호출하는걸로 변환되었다.
render(function App(), document.getElementById("root"));
```

### React 컴포넌트와 상태 관리

**클래스형 컴포넌트**

클래스형 컴포넌트는 라이프사이클이 존재하며 상태를 생성자에서 초기화 할 수 있다. React.Component 를 상속받아서 render 함수를 무조건 구현해줘야한다.

```jsx
import React from "react";
import ReactDOM from "react-dom";

class Hello extends React.Component {
  constructor(props) {
    super(props);
  }

	this.state = {
		count: 1;
	}

	componentDidMount () {	
		this.setState({ count: this.state.count + 1 });
	}

  render() {
    return <p>안녕하세요 !</p>;
  }
}
```
상태를 변경하기 위해서는 setState 함수를 호출하고 setState 를 호출하지 않고 변경하면 리렌더링이 되지 않는다. 변경하기 위해서는 꼭 setState 함수를 호출해야 한다.


**함수형 컴포넌트**


```jsx
function App() {
	let x = 10;
  return (
    <div>
      <h1>상태</h1>
      <Hello />
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

### Hooks 개념 및 예제

```jsx
import React, { useState } from "react";
import ReactDOM from "react-dom";

class Hello extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return <p>안녕하세요 !</p>;
  }
}

function App() {
  const [counter, setCounter] = useState(1);

  return (
    <div>
      <h1 onClick={() => setCounter(counter + 1)}>상태 {counter} </h1>
      <Hello />
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

초기에 함수형 컴포넌트는 상태를 갖지 못하는 컴포넌트였다. 함수가 호출될 때마다 상태가 초기화되기 때문에 유지가 될 수 없었는데 **Hooks** 가 등장하면서 함수형 컴포넌트도 상태를 관리할 수 있게 되었다.

최상위에서만 Hook을 호출해야 한다. 조건문에서 호출한다거나 최상위가 아닌 부분에서 호출 할 경우 배열의 hook index 에 문제가 발생하여 의도한대로 나오지 않을 수 있다.

함수형 컴포넌트는 Class 컴포넌트처럼 라이프사이클을 가질 수 없지만 useEffect 라는 Hook 을 통해 라이프사이클처럼 구현 할 수 있다.
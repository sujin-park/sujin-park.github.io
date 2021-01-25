---
title: '[우아한테크러닝] React & TypeScript 3기 - 6회차'
date: 2020-09-28 23:00:00
category: 'study'
draft: false
---

우아한테크러닝 React & TypeScript 3기 - 6회차
========================

## 컴포넌트 설계


렌더링 관련된 코드가 길어지면 가독성이 좋지 않고 확장성이 떨어지기 때문에 컴포넌트를 분리하는 것이 좋다.
아키텍쳐의 중요한 요소는 네이밍과 컴포넌트를 분리하는 것.


컴포넌트를 분리 전

```jsx
  return (
    <div>
      <header>
        <h1>React and TypeScript</h1>
      </header>
      <ul>
        { sessionList.map((session) => (<li>{session.title</li>)))}
      </ul>
    </div>
  );
};

export default App;
```

컴포넌트 분리 후

SessionItem 이라는 컴포넌트를 생성하고 title 을 props 으로 받아서 렌더링을 해주면 list 와 item 이 분리가 될 수 있다.
```jsx
return (
    <div>
      <header>
        <h1>React and TypeScript</h1>
      </header>
      <ul>
        {sessionList.map(session => (
          <SessionItem title={session.title} />
        ))}
      </ul>
    </div>
  );
};
```


### 클래스 컴포넌트와 함수형 컴포넌트의 차이

함수형 컴포넌트를 선호하는 이유

- 코드양이 적어서
- 부가적인 코드들이 많이 들어간다.
- 함수형 컴포넌트의 단점이었던 상태를 hook 을 통해 단점을 없앨 수 있게 되었다.

Hook이 탄생한 동기가 된 문제 중의 하나는 생명주기 class method 가 관련이 없는 로직들은 모아놓고, 관련이 있는 로직들은 여러개의 메서드에 나누어 놓는 경우가 자주 있기 때문에 함수형 컴포넌트에서도 할 수 있게끔 하고자 나왔다.

```jsx
import React from "react";

const SessionItem = ({ title }) => <li>{title}</li>;

class ClassApp extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      displayOrder: "ASC"
    };
  }

  // arrow function 은 그때마다 this binding 이 다르지 않기 때문에 많이 사용한다.
  onToggleDisplayOrder = () => {
    this.setState({
      displayOrder: this.state.displayOrder === "ASC" ? "DESC" : "ASC"
    });
  };

  render() {
    return (
      <div>
        <button onClick={this.onToggleDisplayOrder}>정렬</button>
        여기여기..
      </div>
    );
  }
}

// hooks 로 변경해서 상태를 가질 수 있게 된다.
const App = (props) => {
  const [displayOrder, toggleDisplayOrder] = React.useState("ASC");
  const { sessionList } = props.store;
  const orderedSessionList = sessionList.map((session, i) => ({
    ...session,
    order: i
  }));

  // 이벤트 핸들러
  const onToggleDisplayOrder = () => {
    toggleDisplayOrder(displayOrder === "ASC" ? "DESC" : "ASC");
  };

  // 상태가 아닌 부분을 변경하는 onToggleDisplayOrder 을 아무리 호출해도 바깥쪽으로 이펙트를 줄 수 없어서
  // 리렌더링이 될 리가 없다!
  return (
    <div>
      <header>
        <h1>React and TypeScript</h1>
      </header>
      <p>전체 세션 갯수 : 4개 {displayOrder}</p>
      <button onClick={onToggleDisplayOrder}>정렬</button>
      <ul>
        {orderedSessionList.map((session) => (
          <SessionItem title={session.title} />
        ))}
      </ul>
    </div>
  );
};

export default App;
```

```jsx
useEffect(() ⇒ {

	// 함수를 return 해주면, 이 컴포넌트가 UI 에서 사라질 때 return 에 있는 함수를 호출해준다.
	// 정의한 것들을 해제하라고 공식문서에도 나와있음. clean up.
	// useEffect 의 return 부분에서 해제하는 대상은 메모리 또는 hook 등등 변수, 객체들이 대상은 아니다.
	return () => {
		// mark and sweep 해주는 알고리즘을 바껴서 더이상 참조하고있지않는 객체를 GC 로 날려버릴 수 있다.
		// Concurrent 방식
		// 일반 객체, 클로저 등등 다 GC 가 일어난다.
	}
})
```

함수가 실행 될 때마다 useEffect 에 전해준 함수를 호출해주는 hook 이다.

dom 에 렌더링이 끝났는지를 알 수 있어서 그 이후에 할 일을 정해줄 수 있다.

### 비동기와 Generator

```jsx
generator : function* foo() {}

비동기 함수 : async function bar() {}
```

**동기 / 비동기**

```jsx
const x = 10;
const y = x * 10;
```

x 라는 변수의 값이 확정되기 이전에 변수 dependency 로 인해 두번째 코드는 제대로 된 평가가 될 수 없다.


```jsx
const x = () => 10;
const y = x() * 10;
```

x 함수가 호이스팅 되면서 평가가 지연 될 수 있다. 지연평가가 가능하다는 것으로 옵저버블 같은 것을 구현할 수 있다.


```jsx
const p = new Promise(function(resolve, reject) {
	// resolve 호출되면 then 에 전달된 함수를 호출할 수 있다. 지연 호출을 할 수 있다. 
	setTimeout(() => {
		resolve("1");
	}, 1000);
});

p.then(function(z) {
	console.log(z);
});
```

### Generator

---

```jsx
function* makeNumber() {
	let num = 1;
	while(true) {
		yield num++;
	}
}

const i = makeNumber(); // 실행될 준비만 하고, i 에게 실행될 메소드 전달.

i.next(); // next 를 호출하면 yield 가 있을 때까지 실행을 긷

console.log(i); // Generator 객체를 준다. (Iterator 라고도 한다.) 코루틴이라는 함수의 구현체.

// 입력값을 받고, 계산을 한 후에 값을 return 하는 것을 함수.
// return 을 안하면 undefined return.
function xyz(x) {

}

xyz();

```



### 비동기를 동기적으로 써보자.

```jsx
const delay = ms => new Promise((resolve) => setTimeout(resolve, ms));

function* main() { // generator.
	console.log('시작');
	yield delay(3000);
	console.log('3초 뒤');
}

const it = main(); // main 함수를 호출해서 받은게 iterator.
const { value } = it.next(); // Promise 객체가 value 에 담긴다.

value.then(() => {
	it.next();
});

delay(3000).then(() => {
	console.log('3초 뒤');
}); // Promise 객체 반환.
```



### 커뮤니케이션에 대해


강사님이 말씀해주신 말들 중에서 좋은 내용이 있어서 정리해두려고 한다.

서로간의 정보를 주고받기 위한 약속인 "프로토콜"이 커뮤니케이션에서는 가장 중요하다고 생각한다. 프로토콜이 안맞는 경우가 많고, 프로토콜이 안맞는걸 의식하지 못하는 사람들도 많다. 내가 A 라는 것을 알고 있고 이걸 가르쳐주려고 할 때, 상대방이 얼마나 알고 있는지 얼마나 이해할 수 있는지에 대해서 어느정도 파악을 하고 있어야 하고 상대 레이어에 맞는 용어와 내용으로 변환해서 말을 해줘야 한다.

사람은 못알아듣고 있다는 것을 상황적으로 말하기가 힘들어서 그 상황에서 알아들은 것처럼 넘어갈 수도 있기 때문에 일방적인 소통을 하려고 하면 안된다. **소프트 스킬 중에 가장 기본 !** 이라고 할 수 있다.
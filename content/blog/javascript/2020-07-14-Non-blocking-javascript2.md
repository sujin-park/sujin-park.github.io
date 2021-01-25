---
layout:     post
title:      "[Javascript] Code spitz 85 [Non blocking Javascript] - 2강"
date:       2020-07-14 22:00:00
author:     "sujin-park"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
multilingual: false
tags:
    - Javascript
---
Code spitz 85 [Non blocking Javascript] - 2강
========================

이전보다 개인적으로 공부 할 시간이 남게 되어서 이전에 스터디 참여했던 Code spitz 85 [Non blocking Javascript] 를 복습하려고 한다.
당시에 회사 다니면서 어렵게 참여해서 들었던 시간을 헛되게 넘기고 싶지 않아서 이번에 복습을 제대로 해봐야겠다. 2강부터 다시 복습하게 된 이유는...가장 열심히 들었고 이 수업 중 가장 많이 이해했던 강의다. 이번엔 꼭 제대로 공부해봐야겠다.

### Concurrency (동시성)

---

- 마치 동시에 하는 것처럼 짧고 빠르게 하는 것
- 하나의 task 만 하나의 메모리를 접근 할 수 있다.
- task A, B 가 둘다 접근해야 한다면 시점을 다르게 해서 접근한다.

### Parallelism (병행성, 병렬성)

---

- 동시에 task 를 진행하는 것
- task 에 할당되는 각각의 worker가 각자 메모리를 쓰고 있고 각자 진행됨
- 물리적으로 2개의 프로세스가 존재해야 task 를 동시에 진행할 수 있음
- 동시에 하나의 메모리를 공유하게 되면 문제가 생길 수 있음
- 하나의 task 가 메모리를 할당하고 있으면, 다른 task 는 좌절, 탈취, 대기를 할 수 있다.

    ( ex. java는 Object 에 wait가 있어서 대기를 할 수 있음 )

- Shared memory, Atomic 으로 인해 이러한 문제가 생긴다.

### Javascript Concurrency

---

**engine work (rendering...)** 

- 자바스크립트 코드에 영향을 끼치는 것들 - 1 thread 를 사용
- 자바스크립트 코드에 영향 X - multi thread 를 사용

check queue ← calback queue ( network, timer, message, dom event : parallelism 으로 되어있음) 

run JS - 자바스크립트를 빠르게 실행이 될 수 있게 작성 해야 함 (engine work, check queue는 우리가 할 수 없음)

- 브라우저는 싱글스레드라고 하는건 engine work, check queue, run JS 부분만 싱글스레드고 나머지는 멀티스레드로 task 를 병행으로 처리하고 있다.
- 스레드 - 생산자를 한 명만 두고 있는 게 **파이프패턴**

### setTimer

---

```javascript
const Item = class {
	time; // 시간
	block; // 실행 될 함수
	constructor (block, time) {
		this.block = block;
		this.time = time + performance.now() // performance.now() 브라우저가 시작 된 이후의 몇초가 지났는지
	}
}

const queue = new Set; // 배열은 사실상 값만 저장하기때문에, 중복되지않은 객체를 저장하고 싶다면 Set을 사용ㅇ

// check queue function
const f = time => {
	queue.forEach(item => { // ES6부터는 forEach 는 복사본을 돌리기 때문에 delete를 해도 무방함
		if (item.time > time) return
		queue.delete(item)
		item.block()
	})
	requestAnimationFrame(f)
}
requestAnimationFrame(f)

//
const timeout = (block, time) => queue.add(new Item(block, time))

timeout(_=> console.log("hello"), 1000)
```

**알아야 하는 것**

1. event loop ?
2. event loop 초기하거나 달성할 수 있는지 ?

### Non Blocking For

---

```javascript
const working =_=> {} // ES6 화살표함수는 인자가 없으면 () 생략하고 _를 할 수 있음

for (let i = 0; i < 100000; i++) working()

const nbFor = (max, load, block) => {
	let i = 0 // i count 를 max-1까지 돌리기
	const f = time => {
		let curr = load
		while(curr-- && i < max) { // 최대 100번만 돌 수 있고, max보다 i가 더 크면 아예 못돌게 작성
			block()
			i++
		}
		console.log(i)
		if (i < max - 1) timeout(f, 0) // 100번만 돌고, engine 에게 다시 돌려줘서 engine work 를 할 수 있다. (제어권 돌려줌)
		// timeoout function 을 사용해서 requestAnimationFrame 을 숨ㄱㅁ
	}
	timeout(f, 0)
}

nbFor(100, 10, working)
```

**위 설명**

- i 는 함수를 실행할 때, 0 으로 초기화 되며 함수를 실행하는 동안 상태가 보존된다.

    → closer 패턴

- 함수의 캡슐화, 지연 실행을 적용한 함수

### Generator

---

```javascript
// 메모리를 할당하고 있지 않아도 무한으로 만들 수 있다.
const infinity = (function *(){ // *를 붙이면 suspend 라는 구간을 만든다.
	let i = 0
	while (true) yield i++ // yield 를 호출하면 suspend. yield 가 호출되면서 멈춤.
})() // iterator 바로 실행 

console.log(infinity.next()) // iterator 의 객체로 next 호출 가능
// next 를 호출할 때 마다 yield 에서 다시 호출되면서 resume
```

1. iterator, iterable, generator 공부하기
2. suspend / resume

```javascript
const gene = function*(max, load, block) {
	let i = 0, curr = load
	while (i < max) { // 다른 서브 루틴을 만들지 않고, 하나의 루프로 작업
		if (curr--) {
			block()
			i++
		} else {
			curr = load
			console.log(i)
			yield// suspend 걸어서 외부에 제어를 위임한다.(반제어)**
		}
	}
}
```

→ 제어 구조를 가지고 있지만, yield (suspend 걸기) 하면 제어의 일부를 바깥으로 위임하여 하고싶은 일을 바깥에서 할 수 있다. (제어 역전의 일부)

제어 문은 복사해서 써 올 수 밖에 없음. 재활용을 할 수가 없다.

명령**문 ( if, while, ...)** 으로 만들면 재활용을 하지 못하고 실행되면 사라짐. 그러므로 값으로 만들어야 함. (값은 메모리에 저장되어 있기 때문에)

```javascript
const nbFor = (max, load, block) => {
	const iterator = gene(max, load, block) // iterator : **value, done** 이 있는 object 를 return 함
	const f =_=> iterator.next().done || timeout(f)
	timeout(f, 0) //timeout 분리 성공
}

nbFor(100, 10, working)
```

### Promise

---

- promise를 return 해보면, then 을 언제 할 것 인지를 제어할 수 있다.
- promise 와 generator 를 같이 사용하면 제어 역전이 가능하다.

```javascript
const gene2 = function*(max, load, block) {
	let i = 0, curr = load
	while (i < max) {
		yield new Promise(res => { // Promise return
			let curr = load
			while (curr-- && i < max) {
				block()
				i++
			}
			coonsole.log(i)
			timeout(res, 0) // then 에 대한 통제권을 바깥에서 가지게 돔
		})
	}
}

// 제어권은 다 Promise 에 가있고, 아래 함수는 실행만 해주는 함수 (단순 실행)
// Generator Co pattern
const nbFor = (max, load, block) => {
	const iterator = gene2(max, load, block)
	const next = ({value, done}) => done || value.then(v => next(iterator.next())) // yield 해소 부분
	next(iterator.next()) 
}
```

---
title: '[우아한테크러닝] React & TypeScript 3기 - 7회차'
date: 2020-09-30 23:00:00
category: 'study'
draft: false
---

우아한테크러닝 React & TypeScript 3기 - 7회차
========================

### 컴포넌트 아키텍쳐

컴포넌트가 비즈니스 로직과 관계성을 가지고 있는지 없는지에 따라서 presentational component, container component 로 구분지을 수 있다. 외부와 관계가 있다면 container component, 단순하게 props 을 받아서 보여주기만 한다면 presentational component가 될 수 있다.

**container component**에서 상태를 통해 로직을 구현하거나 사용자의 흐름과 비즈니스 로직을 구현할 수 있다.

**containers/index.ts**

import 를 containers/index.ts 에 넣어주고 중개해주는 역할을 한다.

일종의 Encapsulation 을 하는 것이다. 

```jsx
import {
  NotificationContainer,
  OrderStatusContiner,
  MonitorControllerContainer
} from "./containers";
import { Typography } from "antd";
```

Container 컴포넌트 접미사로 Container 를 붙이는 것을 고민을 하였지만, 붙여야 jsx 에서 확연히 보이기 때문에 붙이는걸로 결정을 하였다.

** 배민은 파일명 자체에 이 컴포넌트가 하는 역할에 대한 내용을 전부 적기로 결정을 하였다. 파일명이 조금 길어도 이 파일이 계속 이 폴더에 있을거라는 보장도 없고, IDE 에서 파일을 찾기가 힘들다. **

components

ㄴ products

ㄴ ProductsList.vue

컴포넌트에서는 interface, type, generic 정도만 사용을 한다.

```jsx
export interface OrderStatusProps {
  showTimeline: boolean;
  success: number;
  failure: number;
  successTimeline: ITimelineItem[];
  failureTimeline: ITimelineItem[];
}

class OrderStatus extends React.Component<OrderStatusProps> {}

export const OrderStatusContiner = connect(mapStateToProps)(OrderStatus);
```

```jsx
type Person = {
	name: string;
	age: number;
	job?: [];
}

interface Human {
	name: string;
	age: number;
	job?: [];
}

// 이 상황은 TypeScript 가 잡아 줄 수 없다. 어떤 값을 입력할지 모르기때문에
const p: Person = JSON.parse(prompt('객체를 입력해주세요.'));

{
	name: 'Kim',
	age: 24,
}
```

TypeScript 는 compile time 에 type 을 체크해주기 때문에 Runtime 에서 Type 이 명확하지 않은 값을 넣어주게 된다면 Runtime 에서는 type 을 체크 못해서 에러가 발생한다.

### interface vs type

- interface
    - 상속 지원
    - Union type 지원하지 않음
- Type
    - 상속 불가능
    - Union type 지원

Union type이란?
- 2개 이상으로 입력된 타입에 대해 하나의 타입으로 정의할 수 있는 타입으로 둘 중 하나의 타입만 유효하면 할당이 이루어진다.
```jsx
type box = number | string;

let b: box = 10;

box = '10';
```

### Generic

컴파일 타임에 동작하는 것.

```jsx
function identity(arg: any): any {
	return arg;
}

function identity<T>(arg: T): T {
	return arg;
}

let output = identity<string>("myString");
//         = let output: string
```

### Redux

```jsx
import { connect } from "react-redux";
```

HoC 로 되어있는 connect 라는 helper 함수이다.

index.tsx 파일에서 store 를 주입시켜줬을 때, 컴포넌트에서 각자 필요한 부분만 사용을 해야하기 때문에 connect 가 연결을 시켜준다.

```jsx
export const OrderStatusContiner = connect(mapStateToProps)(OrderStatus);
// connect 의 mapStateToProps state를 props로 맵핑시켜주는 함수를 connect 에 주면
// OrderStatus 의 props 로 mapStateToProps 를 넣어준다.
```

→ 이렇게 하는 부분을 사람들이 다 복잡해하고 어려워한다.

→ 그래서 hook 이 나와서 조금 괜찮아짐..

### React Redux

redux - hooks 로 만들어보는 방법 

```jsx
// store 의 데이터를 가져오는 hooks.
useSelector()

// dispatch 가져오는 hooks
useDispatch()
```

**주의사항**

codesandbox 에서 실제 api 를 호출 할 때, 서버가 작기 때문에 트래픽을 많이 못견뎌서 조심해야 한다.

→ codesandbox fork 받아서 redux hook 으로 바꿔봐도 좋을 것 같다!

pure component도 바깥쪽의 상태나 로직과 관련이 없다면 자기 자체의 상태와 hook, default option 등을 가지고 있을 수 있다.

배민에서 사용하는 패턴 중 하나. Maybe 라는 컴포넌트.

test 속성을 판단해서 참이면 렌더링을 하고, 그렇지않으면 렌더링을 하지 않는 컴포넌트.

아래 컴포넌트 형태로 if-else 를 만들 수도 있다!

```jsx
import * as React from "react";

interface IMaybeProps {
  test: boolean;
  children: React.ReactNode;
}

export const Maybe: React.FC<IMaybeProps> = ({ test, children }) => (
  <React.Fragment>{test ? children : null}</React.Fragment>
);
```

jsx 에서 javascript conditional 이 많이 나오게 되면 readablity 가 떨어지기 때문에 Maybe 컴포넌트에 조건부 렌더링을 넣어준다.

**AS-IS**

```jsx
{
	this.props.showTimeline ?
	<TinyChart
    source={this.props.successTimeline.slice(
      this.props.successTimeline.length - 10
    )}
  /> : null
}
```

**TO-BE**

```jsx
<Maybe test={this.props.showTimeline}>
  <TinyChart
    source={this.props.successTimeline.slice(
      this.props.successTimeline.length - 10
    )}
  />
</Maybe>
```

### api

axios 를 Promise 로 wrapping.

```jsx
export function fetchNumberOfSuccessfulOrder(): Promise<
  INumberOfSuccessfulOrderResponse
> {
  return new Promise((resolve, reject) => {
    axios
      .get(endpoint.orders.request.success({ error: true }))
      .then((resp: AxiosResponse) => resolve(resp.data))
      .catch((err: AxiosError) => reject(new ApiError(err)));
  });
}
```

```jsx
function* watchFetchOrderTimeline() {
  yield takeLatest(getType(Actions.showOrderTimelineChart), fetchOrderTimeline);
}
```

takeLatest 를 통해 사용하면 가장 마지막에 요청된 것만 호출을 하는 effector 함수가 있다.
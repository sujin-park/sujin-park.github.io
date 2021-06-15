---
title: 'React 컴포넌트 테스트 환경 구축하기 (testing-library)'
date: 2021-06-15 22:00:00
category: 'react'
draft: false
---


보통 유틸성 함수를 만들 때는 테스트 코드를 작성하였지만 컴포넌트 테스트 코드는 작성해본적이 없었다.

작년에 컴포넌트 테스트 코드에 대해서 중요성을 느끼게 해준 FE Conf Korea 2020 - 최수형님의 [프론트엔드에서 TDD가 가능하다는 것을 보여드립니다.](https://www.youtube.com/watch?v=L1dtkLeIz-M) 라는 강연을 듣고 실행에 옮기려고 할 일에 넣어두었는데 2021년이 되어서야 작성해보게 되었다.

테스트 코드 작성을 위해 사용하게된 [testing-library](https://testing-library.com/) 는 DOM 테스트 라이브러리로 Jest, Mocha + JSDOM 또는 실제 브라우저와 같은 DOM API를 제공하는 모든 환경에서 작동한다. Jest를 기반으로 UI테스트를 지원해주는 DOM Testing Library에 React Component를 위한 API들이 추가된 것이다.


## 특징

- 모든 테스트를 DOM 위주로 진행하여 컴포넌트의 props 나 state 를 조회하는 일이 없다.
- testing-library는 컴포넌트를 리팩토링 할 때 내부 구조 및 네이밍은 많이 바뀌어도 실제 작동 방식은 크게 바뀌지 않는다는 점을 중요시 여겨서, 컴포넌트의 기능이 똑같이 작동한다면 컴포넌트의 내부 구현 방식이 많이 바뀌어도 테스트가 실패하지 않도록 설계되었다.
- React, Vue, Angular 와 함께 사용할 수 있고, e2e test library 인 Cypress 내에서 dom-testing query 를 사용할 수 있는 플러그인 또한 구현 되어있다. 또한, React Native 를 위한 구현도 되어있다.

<br>

## 환경 설정


### 1. 의존성 설치

> 우선 create-react-app 으로 프로젝트를 구성한다면 jest 와 @testing-library 설치에 대해서는 넘어가도 된다.

create-react-app 을 사용하지 않고 bundler 로 프로젝트를 구성한다고 가정하고 필요한 라이브러리를 설치한다.

  ```sh
  $ yarn add -D jest @testing-library/react @testing-library/jest-dom
  ```

그리고 아주 간단하게 테스트해볼 스타일과 컴포넌트를 아래와 같이 작성한다.

  ```jsx
  // App.scss

  main {
    background-color: pink;
  }
  ```

  ```jsx
  // App.jsx

  import React from 'react';
  import './App.scss';

  const App = () => {
    return (
      <main>
          <h1>Hello, React!</h1>
      </main>
    );
  };

export default App;
```

<br>

### 2. jest 설정

**src/setupTests.js 생성**

테스트 파일이 실행되기 전에 테스트 프레임워크를 구성하거나 설정하기 위해 실행하고자 하는 모듈을 setupTests.js 에 import 한다.

```jsx
// src/setjpTests.js

// import '@testing-library/react/cleanup-after-each';
import '@testing-library/jest-dom/extend-expect';
```

이전에는 create-react-app 으로 프로젝트를 설정하면 `@testing-library/jest-dom/extend-expect` 와 함께 `@testing-library/react/cleanup-after-each`가 import 되었는데 [deprecated](https://github.com/testing-library/react-testing-library/pull/598) 되면서 이제는 module 이 자동으로 import 되지 않는다.

위와 같이 setupTests.js 파일을 생성하고 `setupFilesAfterEnv` 경로를 지정해주면 된다.

```jsx
module.exports = {
  moduleNameMapper: {
    '^.+\\.(css|less|scss)$': 'babel-jest'
  },
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['./setupTests.js']
}
```

setupTests.js 로 따로 분리하지 않아도 된다면 아래와 같이 setupFilesAfterEnv 배열에 추가해주면 된다.

```js
// jest.config.js 

module.exports = {
  moduleNameMapper: {
    '^.+\\.(css|less|scss)$': 'babel-jest'
  },
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: [
    '@testing-library/jest-dom/extend-expect'
  ]
}
```

<br>

### 3. 테스트 코드 작성

```jsx
// App.test.js

import { render } from "@testing-library/react";
import App from './App';
import React from 'react';

describe('<App />', () => {
  it('render App component', () => {
    const { getByText } = render(<App />);
  
    const text = getByText('Hello, React!');
    expect(text).toBeInTheDocument();
  })
})
```

<br>

### 4. 실행


여기까지 설정하고 테스트를 실행하면 정상적으로 성공이 되는 것을 확인할 수 있다.

<img width="372" alt="_2021-06-05__1 18 17" src="https://user-images.githubusercontent.com/29244798/122057049-2b453600-ce25-11eb-98b7-005700d9f6ca.png">


jest 는 기본적으로 `rootDir/__test__` 에 있는 테스트들을 대상으로 하기 때문에 다른 디렉토리에 있는 테스트를 실행하고 싶다면 `testMatch` option 을 추가해주면 된다.

```jsx
// jest.config.js

module.exports = {
  'moduleNameMapper': {
    '^.+\\.(css|less|scss)$': 'babel-jest'
  },
  'testEnvironment': 'jsdom',
  'setupFilesAfterEnv': [
    '@testing-library/jest-dom/extend-expect'
  ],
  'testMatch': ["**/components/*.test.(js|jsx)"],
}
```

코드는 아래 저장소에서 확인해볼 수 있다.

[https://github.com/sujin-park/mash-up-react/tree/master/react-webpack](https://github.com/sujin-park/mash-up-react/tree/main/react-webpack)

<br>

### 참고

- [https://testing-library.com/docs/react-testing-library/setup/](https://testing-library.com/docs/react-testing-library/setup/)
- [https://velog.io/@velopert/react-testing-library](https://velog.io/@velopert/react-testing-library)
- [https://stackoverflow.com/questions/62852435/typescript-react-testing-library-sidebaritem-refers-to-a-value-but-is-bei](https://stackoverflow.com/questions/62852435/typescript-react-testing-library-sidebaritem-refers-to-a-value-but-is-bei)
---
title: 'React-Router hooks에 대해'
date: 2021-05-24 22:00:00
category: 'react'
draft: false
---

스터디한 강의에서는 use-react-router 라는 third party library 를 사용하는 예시까지만 나왔는데, 이제는 React-Router 에서 제공하는 Hooks 가 있을거라 생각되어 [공식 문서](https://reactrouter.com/web/api/Hooks)를 들어가보았고 Hooks 가 업데이트 된 것을 보았다.

### useHistory

history 객체를 반환해주는 useHistory hooks 를 아래와 같이 사용할 수 있다.

```jsx
import { useHistory } from 'react-router-dom';

function HomeButton() {
  const history = useHistory();

  function handleClick() {
    history.push('/');
  }

  return (
    <button type="button" onClick={handleClick}>홈으로</button>
  );
}
```

### useLocation

location 객체를 반환해주는 useHistory hooks 를 아래와 같이 사용할 수 있다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import {
  BrowserRouter as Router,
  Switch,
  useLocation
} from 'react-router-dom';

function usePrintLocation() {
  const location = useLocation();
  
  console.log(location.pathname);
  return null;
}

function App() {
  usePrintLocation();
  return <Switch>...</Switch>;
}

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  node
);
```

### useParams

- URL Params 를 간편하게 구조 분해 할당을 통해 받을 수 있다.
- Context API 를 사용하는 Hooks 로 `<Route>` 컴포넌트의 children component 에 추가되어야 정상적으로 값을 받을 수 있다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import {
  BrowserRouter as Router,
  Switch,
  Route,
  useParams
} from 'react-router-dom';

function BlogPost() {
  let { slug } = useParams();
  return <div>Now showing post {slug}</div>;
}

ReactDOM.render(
  <Router>
    <Switch>
      <Route exact path="/">
        <HomePage />
      </Route>
      <Route path="/blog/:slug">
        <BlogPost />
      </Route>
    </Switch>
  </Router>,
  node
);
```

### useRouteMatch 사용 예시

- URL Pattern Match 을 통한 처리를 하고자 하는 경우에 사용할 수 있다.
- URL Pattern 을 체크하는 기능이기 때문에 Context 에 얽매이지 않고 광범위하게 사용할 수 있다.

기존에는 Route 컴포넌트의 render 함수에서 match 를  통해 URL Pattern match 를 처리하였다.

```jsx
import { Route } from 'react-router-dom';

function BlogPost() {
  return (
    <Route path='/blog/:slug' render={({ match }) => {
        return <div />;
	    }}
		/>);
}
```

useRouteMatch 를 사용하면 아래와 같이 Context 에 얽매이지 않고 사용할 수 있다.

```jsx
import { useRouteMatch } from 'react-router-dom';

function BlogPost() {
  let match = useRouteMatch("/blog/:slug");

  return <div />;
}
```


### 참고
[https://reactrouter.com/web/api/Hooks](https://reactrouter.com/web/api/Hooks)
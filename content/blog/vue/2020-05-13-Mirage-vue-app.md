---
layout:     post
title:      "[etc] API Mocking Library - Mirage 설정하기 (Vue.js)"
date:       2020-05-13 22:50:00
author:     "sujin-park"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
multilingual: false
tags:
    - etc
---

# API Mocking Library Mirage 설정하기 (Vue.js)

### Mirage JS 소개

Vue.js 에서 Mirage JS 로 API Mocking  당장 호출 가능한 API가 없어도 Javascript application 을 빌드, 테스트 할 수 있는 API Mocking 라이브러리다.

프론트엔드 웹 애플리케이션을 개발 할 때 API 개발 일정과 프론트엔드 일정이 동시에 진행 될 때도 있고, API 구현 전 설계 단계에서 API를 돌려보면서 이견을 조율해야 할 때도 있다.

이때 필요한 것이 바로 API Mocking이다. 이 API Mocking을 이용해서 프론트엔드 웹 애플리케이션이나 모바일 애플리케이션에서 실제 작동 여부와 상관없이 시뮬레이션을 할 수 있고, 이 결과를 이용해 API 개발자에게 피드백을 줄 수 있다.

### Installation

```markdown
# Using npm
npm install --save-dev miragejs

# Using Yarn
yarn add --dev miragejs
```

### The database

Mirage JS는 데이터베이스에 직접 액세스하지 않지만 Mirage의 ORM을 통해 데이터베이스와 상호 작용을 한다.

```javascript
// server.js

import { Server } from "miragejs";

export function makeServer({ environment = "development" } = {}) {
  let server = new Server({
    environment,

    seeds(server) {
      server.db.loadData({
        movies: [ // dummy data
          { text: "Buy groceries", isDone: false },
          { text: "Walk the dog", isDone: false },
          { text: "Do laundry", isDone: false }
        ]
      });
    },

    routes() {
      this.urlPrefix = 'https://my.api.com'; // url prefix 설정 

      this.get("/movies", (schema, request) => {
        return schema.db.movies
      })
      
      this.get("/movies/:id", (schema, request) => {
        let id = request.params.id
      
        return schema.db.movies.find(id)
      })
      
      this.post("/movies", (schema, request) => {
        let attrs = JSON.parse(request.requestBody)
      
        return schema.db.movies.create(attrs)
      })
      
      this.patch("/movies/:id", (schema, request) => {
        let newAttrs = JSON.parse(request.requestBody)
        let id = request.params.id
        let movie = schema.db.movies.find(id)
      
        return movie.update(newAttrs)
      })
      
      this.delete("/movies/:id", (schema, request) => {
        let id = request.params.id
      
        return schema.db.movies.find(id).destroy()
      })
    }
  });

  window.server = server;

  return server;
}
```

server 인스턴스를 반환하는 makeServer 함수를 호출한다.

여기까지만 해도 바로 시뮬레이션 해볼 수 있다.

```javascript
// main.js

import Vue from "vue";
import VueRouter from "vue-router";
import App from "./App.vue";
import { makeServer } from "./server";
import { Server, Response } from "miragejs";
import Todos from "./components/Todos";

Vue.config.productionTip = false;
Vue.use(VueRouter);

makeServer();

const router = new VueRouter({
  mode: "history",
  routes: [
    { path: "/", component: Todos },
  ]
});

new Vue({
  router,
  render: h => h(App)
}).$mount("#app");
```

### 빌드 할 때 Mirage JS 제외하기

Mirage는 개발 도구이므로 production 환경에서는 유저가 불필요한 리소스를 다운로드하지 않게끔 빌드에서 제외하도록 구성해야한다.

1. null-loader 설치
2. vue.config.js 수정
3. API mocking 이 더이상 존재하지 않기 때문에 makeServer 함수 호출하지 않게끔 변경

```javascript
// vue.config.js

module.exports = {
  chainWebpack: config => {

    if (process.env.NODE_ENV === "production") {
      config.module
        .rule("exclude-mirage")
        .test(/node_modules\/miragejs\//)
        .use("null-loader")
        .loader("null-loader");
    }
  }
};
```

```javascript
// main.js

if (process.env.NODE_ENV === "development") {
  makeServer()
}
```

### 참고

[https://miragejs.com/](https://miragejs.com/)

[https://miragejs.com/quickstarts/vue/development/](https://miragejs.com/quickstarts/vue/development/)
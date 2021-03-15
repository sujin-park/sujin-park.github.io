---
title: 'Vue Drag & Drop 구현할 기능에 적절한 library 사용하기'
date: 2020-08-05 22:00:00
category: 'vue'
draft: false
---


최근에 Vue.js 에서 드래그앤드롭 기능을 구현 할 일이 있어서 이런 저런 library 를 찾아보다가 몇가지 보게 되었다. 간단하게 정리해둔 내용에 대해서 공유를 하면 좋을 것 같아서 블로그에 올리게 되었다.


### Vue.Draggable
https://www.npmjs.com/package/vuedraggable


우선 드래그앤드롭을 구현할 때 가장 먼저 알게 된 Vue.draggable 이다. npm Weekly Downloads 가 36만이 넘어서 다른 라이브러리들에 비해 높은 편이다. Browser Support에 대해서 검색을 해봤는데 결과가 따로 안나왔지만 Edge 에서는 우선 잘 되는 것을 확인하였다.


**사용법**

npm install
```javascript
yarn add vuedraggable
 
npm i -S vuedraggable
```

use in file
```html
<draggable v-model="array" @start="drag = true" @end="drag = false">
   <div v-for="item in array" :key="item.id"> {{item.name}} </div>
</draggable>
```

기본적인 예제 외에도 많은 옵션을 제공하고 있고, 가장 직관적이라고 생각되었다. 또한, 케이스 정리가 잘 되어있어서 보통의 케이스는 https://sortablejs.github.io/Vue.Draggable/#/simple 여기에서 다 볼 수 있었다.

### vue-draggable-nested-tree

https://www.npmjs.com/package/vue-draggable-nested-tree

vue-draggable-nested-tree 는 depth 가 있는 트리구조를 구성하여 드래그앤드롭 기능을 구현할 때 사용할 수 있다. vue-draggable-nested-tree 는 IE 를 지원한다.

참고: https://www.npmjs.com/package/vue-draggable-nested-tree#template-for-old-browserseg-ie

**사용법**

npm install
```javascript
npm i vue-draggable-nested-tree
```

```html
<template>
Tree(:data="data" draggable cross-tree)
  div(slot-scope="{data, store, vm}")
    template(v-if="!data.isDragPlaceHolder")
      b(v-if="data.children && data.children.length" @click="store.toggleOpen(data)") {{data.open ? '-' : '+'}}&nbsp;
      span {{data.text}}
</template>
```

data는 drag 되는 노드 자체를 가리키고, store 는 tree 를 가리키므로 store.toggleOpen, store.getPureData 같은 제공해주는 Tree methods를 호출할 수 있다.

vm 은 node Vue instance 로 ```vm.level```을 통해 node level 값을 얻을 수 있다.


```javascript
<script>
import { DraggableTree } from 'vue-draggable-nested-tree'

data: [
  {text: 'node 1'},
  {text: 'node 2'},
  {text: 'node 3 undraggable', draggable: false},
  {text: 'node 4'},
  {text: 'node 4 undroppable', droppable: false},
  {text: 'node 5', children: [
    {text: 'node 1'},
    {text: 'node 2', children: [
      {text: 'node 3'},
      {text: 'node 4'},
    ]},
    {text: 'node 2 undroppable', droppable: false, children: [
      {text: 'node 3'},
      {text: 'node 4'},
    ]},
    {text: 'node 2', children: [
      {text: 'node 3'},
      {text: 'node 4 undroppable', droppable: false},
    ]},
  ]},
]
</script>
```

### vue-smooth-dnd

https://www.npmjs.com/package/vue-smooth-dnd

Demo : https://kutlugsahin.github.io/vue-smooth-dnd/#/cards

vue-smooth-dnd 또한 드래그앤드롭을 구현할 수 있고 데모에서 다양한 케이스를 확인해볼 수 있다. Vue.Draggable 이나 vue-draggable-nested-tree 보다 동작 할 때 animation 효과를 다양하게 설정할 수 있는 점이 좋은 것 같다.


### 참고

- https://www.npmjs.com/package/vuedraggable
- https://www.npmjs.com/package/vue-draggable-nested-tree
- https://www.npmjs.com/package/vue-smooth-dnd


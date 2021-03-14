---
title: functionalTemplate
date: 2020-06-03 14:56:51
keyword: Vue
tags: Vue
cover: https://static.nnnnzs.cn/bing/20200603.png
---

在使用iview表格的时候，定制组件，需要用到renderHeader API，看了一下如何传入render函数，结果惊为天人

```javascript
export default {
    name: 'TableRenderHeader',
    functional: true,
    props: {
        render: Function,
        column: Object,
        index: Number
    },
    render: (h, ctx) => {
        const params = {
            column: ctx.props.column,
            index: ctx.props.index
        };
        return ctx.props.render(h, params);
    }
};
```

这是一个对象，可以转换为函数式组件，可以将render函数传入到组件里面，实现传什么渲染什么,同时还能将内容通过props传到组件里面

``` javascript
const renderTemplate = {
  name: "renderTemplate",
  functional: true,
  props: {
    render: Function,
    index: Number,
    scope: Object
  },
  render: (h, ctx) => {
    const params = ctx.props.scope;
    return ctx.props.render(h, params);
  }
};
export default {
  name: "demo",
  components: {
    renderTemplate
  },
  data() {
    return {
      list: [
        {
          text: "123",
          render: (h, { text }) => {
            return h("div", text);
          }
        },
        {
          text: "456",
          render: (h, { text }) => {
            return h("div", text);
          }
        }
      ]
    };
  }
};
```
```Vue
<template>
  <ul>
    <li v-for="(item,index) in list" :key="item.text">
      <renderTemplate :index="index" :render="item.render" :scope="{text:item.text,index}" />
    </li>
  </ul>
</template>
```
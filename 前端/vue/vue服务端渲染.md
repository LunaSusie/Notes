---
title: vue服务端渲染
tags: vue,vue-server-renderer
grammar_cjkRuby: true
---

npm install vue vue-server-renderer --save

# 使用vue-server-render
## 渲染vue组件
```javascript
// 第 1 步：创建一个 Vue 实例
const Vue = require('vue')
const app = new Vue({
  template: `<div>Hello World</div>`
})
// 第 2 步：创建一个 renderer
const renderer = require('vue-server-renderer').createRenderer()
// 第 3 步：将 Vue 实例渲染为 HTML
renderer.renderToString(app, (err, html) => {
  if (err) throw err
  console.log(html)
  // => <div data-server-rendered="true">Hello World</div>
})
```
## 使用一个页面模板

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
```html?linenums
<!DOCTYPE html>
<html lang="en">
  <head><title>Hello</title></head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>
```
```javascript?linenums
const renderer = createRenderer({
  template: require('fs').readFileSync('./index.template.html', 'utf-8')
})
renderer.renderToString(app, (err, html) => {
  console.log(html) // will be the full page with app content injected.
})
```
## 模板插值
```html?linenums
<html>
  <head>
    <!-- 使用双花括号(double-mustache)进行 HTML 转义插值(HTML-escaped interpolation) -->
    <title>{{ title }}</title>
    <!-- 使用三花括号(triple-mustache)进行 HTML 不转义插值(non-HTML-escaped interpolation) -->
    {{{ meta }}}
  </head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>
```
```javascript?linenums
const context = {
  title: 'hello',
  meta: `
    <meta ...>
    <meta ...>
  `
}
renderer.renderToString(app, context, (err, html) => {
  // page title will be "Hello"
  // with meta tags injected
})
```
# 通用代码
## 服务器上的数据响应
每个请求应该都是全新的、独立的应用程序实例。
服务器上“预取”数据("pre-fetching" data) - 这意味着在我们开始渲染时，我们的应用程序就已经解析完成其状态。
数据进行响应式的过程在服务器上是多余的，所以默认情况下禁用。
## 组件生命周期钩子函数
由于没有动态更新，所有的生命周期钩子函数中，只有 beforeCreate 和 created 会在服务器端渲染(SSR)过程中被调用。这就是说任何其他生命周期钩子函数中的代码（例如 beforeMount 或 mounted），只会在客户端执行。
你应该避免在 beforeCreate 和 created 生命周期时产生全局副作用的代码
## 访问特定平台(Platform-Specific) API
对于共享于服务器和客户端，但用于不同平台 API 的任务(task)，建议将平台特定实现包含在通用 API 中，或者使用为你执行此操作的 library。例如，axios 是一个 HTTP 客户端，可以向服务器和客户端都暴露相同的 API。
## 自定义指令
大多数自定义指令直接操作 DOM，因此会在服务器端渲染(SSR)过程中导致错误。有两种方法可以解决这个问题：
* 推荐使用组件作为抽象机制，并运行在「虚拟 DOM 层级(Virtual-DOM level)」（例如，使用渲染函数(render function)）。
* 如果你有一个自定义指令，但是不是很容易替换为组件，则可以在创建服务器 renderer 时，使用 directives 选项所提供"服务器端版本(server-side version)"。
# 源码结构
## 路由
```javascript?linenums
// router.js
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
export function createRouter () {
  return new Router({
    mode: 'history',
    routes: [
      // ...
    ]
  })
}
```
## 通用入口
``` javascript?linenums
//app.js通用入口
import Vue from 'vue'
import App from './App.vue'
// 导出一个工厂函数，用于创建新的
// 应用程序、router 和 store 实例
export function createApp () {
  const app = new Vue({
    // 根实例简单的渲染应用程序组件。
    render: h => h(App)
  })
  return { app }
}
```
## 客户端入口
``` javascript?linenums
//entry-client.js
import { createApp } from './app'
// 客户端特定引导逻辑……
const { app } = createApp()
// 这里假定 App.vue 模板中根元素具有 `id="app"`
app.$mount('#app')
```
## 服务端入口
``` javascript?linenums
//entry-server.js
import { createApp } from './app'
export default context => {
  const { app } = createApp()
  return app
}
```
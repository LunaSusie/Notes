---
title: Vue+Vuex+axios+mockjs做登陆界面
tags: vue,
grammar_cjkRuby: true
---

梳理Vuex+axios+mockjs的整个工作流程。
# 项目创建
项目使用easywebpack-cli创建一个easywebpack-vue项目。
首先介绍一下这个手脚架的启动流程。

![项目结构][1]

framework文件夹使用放全局的东西，其中entry是多页面入口，app.js是单页面入口。component中是加载组件。

![framework文件夹][2]

page文件夹是用来放页面文件的。如果是单页面应用，应该和我下面的差不多。

![page文件夹][3]

# 创建svg图标组件




  [1]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863635837.jpg
  [2]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863726638.jpg
  [3]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863813922.jpg
---
title: Vue+Vuex+axios+mockjs做登陆界面
tags: vue,
grammar_cjkRuby: true
---

梳理Vuex+axios+mockjs的整个工作流程。
项目参考的是这个https://github.com/PanJiaChen/vue-element-admin
# 项目创建
项目使用easywebpack-cli创建一个easywebpack-vue项目。
首先介绍一下这个手脚架的启动流程。

![项目结构][1]

framework文件夹使用放全局的东西，其中entry是多页面入口，app.js是单页面入口。component中是加载组件。

![framework文件夹][2]

page文件夹是用来放页面文件的。如果是单页面应用，应该和我下面的差不多。

![page文件夹][3]

# 创建svg图标组件

![存放图标的文件夹，其实可以任意放，只有后面引入就可以][4]

![easywebpack的配置和原生的有一些区别，建议看文档，这里主要扩展了loader，使用svg-sprite-loader处理上面指定文件夹中的svg图标，而图片处理则过滤掉这个目录][5]

![svg组件vue文件][6]

![svg组件的加载文件和导入svg图标][7]

![全局引用，顺便把element-ui和normalize.css所以一起引用了，mocks一会在说][8]

# 界面创建
界面创建安装参考项目界面来就可以。

![入口，注意引用，下面的store就是在这里引入的][9]

在登陆的事件这里，使用到了this.$store.dispatch分发了一个loginByUsername，然后一切就交给了

![登陆的事件][10]


  [1]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863635837.jpg
  [2]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863726638.jpg
  [3]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863813922.jpg
  [4]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863949530.jpg
  [5]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864001271.jpg
  [6]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864182706.jpg
  [7]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864222793.jpg
  [8]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864264300.jpg
  [9]: https://markdown.xiaoshujiang.com/img/spinner.gif "[[[1513864843558]]]"
  [10]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864740468.jpg
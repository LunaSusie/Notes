---
title: Vue+Vuex+axios+mockjs做登陆界面
tags: vue,
grammar_cjkRuby: true
---

梳理Vuex+axios+mockjs的整个工作流程。
分析项目https://github.com/PanJiaChen/vue-element-admin
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

# 流程梳理

![入口，注意引用，下面的store就是在这里引入的][9]

![登陆的事件中，store分配了一个loginByUsername的action][10]

![我把不必要的都注释掉了，主要看这个user][11]

![loginByUsername，找到了loginByUsername这个action了，它里面使用了一个login方法][12]

![login方法的来源][13]

![login方法是发送请求的，这个请求的定义在utils/request里面][14]

![request里面是定义axios实例][15]

![上面全局引入的mocks][16]

写在最后：登录按钮按下之后，通过Vuex执行loginByUsername这个action，这个action使用axios实例发送login请求，最后mocks拦截这个请求mock数据返回，最后返回到按钮事件里面，显示登录成功。


  [1]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863635837.jpg
  [2]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863726638.jpg
  [3]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863813922.jpg
  [4]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513863949530.jpg
  [5]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864001271.jpg
  [6]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864182706.jpg
  [7]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864222793.jpg
  [8]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864264300.jpg
  [9]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864843558.jpg
  [10]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513864740468.jpg
  [11]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513865028680.jpg
  [12]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513865126736.jpg
  [13]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513865201217.jpg
  [14]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513865245917.jpg
  [15]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513865299769.jpg
  [16]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513865493231.jpg
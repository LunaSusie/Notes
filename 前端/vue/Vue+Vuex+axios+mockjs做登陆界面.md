---
title: Vue+Vuex+axios+mockjs做登陆界面
tags: vue,
grammar_cjkRuby: true
---

梳理Vuex+axios+mockjs的整个工作流程。
# 项目创建
本文使用easywebpack-cli创建项目。
大致项目结构如下：
![enter description here][1]
# 创建页面
登陆界面参考的https://github.com/PanJiaChen/vue-element-admin这个开源项目，界面效果如下图
![enter description here][2]
文件如下：只做一个登陆的界面。
![enter description here][3]
这里顺便总结一下svg图标使用。
在asset文件夹中创建一个icons文件夹存放svg文件。

![icons文件夹][4]

![创建一个svg组件][5]

![icon-svg内容][6]

![index.js内容][7]

![全局引用][8]


  [1]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862554506.jpg
  [2]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862450300.jpg
  [3]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862660795.jpg
  [4]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862720514.jpg
  [5]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862770615.jpg
  [6]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862796952.jpg
  [7]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862816614.jpg
  [8]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/Vue+Vuex+axios+mockjs%E5%81%9A%E7%99%BB%E9%99%86%E7%95%8C%E9%9D%A2-2017-12-21-1513862993937.jpg
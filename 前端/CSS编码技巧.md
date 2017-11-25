---
title: CSS编码技巧
tags: css
grammar_cjkRuby: true
---
# 尽量减少代码重复
```css?linenums
padding: 6px 16px; 
border: 1px solid #446d88; 
background: #58a linear-gradient(#77a0bb, #58a); 
border-radius: 4px; 
box-shadow: 0 1px 5px gray; 
color: white; 
text-shadow: 0 -1px 1px #335166; 
font-size: 20px; 
line-height: 30px;
```
![enter description here][1]

* linear-gradient属性值：用线性渐变创建图像
* HSLA(H,S,L,A)：颜色类型取值
* 互相依赖的值用关系表达式表达出来，例如行高和字体大小之间是关联的
* 字体字号使用百分比或者em单位（em父级关联），也可以使用rem单位（rem根级关联）
* 变更颜色，把半透明的黑色和白色叠加在主色调上，即可产生主色调的亮色和暗色

```css?linenums
padding: .3em .8em; 
border: 1px solid rgba(0,0,0,.1); 
background: #58a linear-gradient(hsla(0,0%,100%,.2),transparent);
border-radius: .2em; 
box-shadow: 0 .05em .25em rgba(0,0,0,.5);
color: white; 
text-shadow: 0 -.05em .05em rgba(0,0,0,.5); 
```

* 修改之后只要修改background-color属性，就可以获得不同颜色的按钮
* 为了易维护，有时候需要把一条css代码拆分为几条
* currentColor属性值
* inherit属性值，继承的使用
# 相信你的眼睛，而不是数值
## 视觉错觉
* 字母的形状在两端都比较整齐，而顶部和底部则往往参差不齐，需要减少顶部和底部的内边距。
# 响应式设计
* 避免不必要的媒体查询
* 媒体查询中使用 em单位取代像素单位。这能让文本缩放在必要时触发布局的变化。
* 使用百分比长度单位取代固定长度。或者视口单位。
* 在较大分辨率下获取固定宽度时，使用max-width而不是width。
* 不要忘记替换元素，比如img、object、video、iframe等，设置一个max-width，值为100%。
* 假如背景图片需要铺满一个容器，不管容器尺寸如何变化，background-size: cover 这个属性可以做到。
* 当图片或者其他元素以行列式进行布局时，让视口决定列的数量。弹性盒布局（flexbox）或者display:inline-block加上常规文本折行都可以实现这一点。
* 在使用多列文本时，指定column-width，而不是column-count。这样它就可以在较小的屏幕上自动显示为单列布局。
# 合理简写
* 列表扩散规则，简写也是会扩散的
# 预处理器
## 使用预处理器的问题
*  css文件体积和复杂度无法控制
* 调试难度增加
* 很多预处理器功能已经添加到标准了
* 好处是，维护方便

[1]: https://www.github.com/loveshullf/Notes/raw/img/%E5%B0%8F%E4%B9%A6%E5%8C%A0/CSS%E7%BC%96%E7%A0%81%E6%8A%80%E5%B7%A7-2017-11-22/1511351247063.jpg
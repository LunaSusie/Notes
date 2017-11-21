---
title: JS面向对象杂记
tags: JavaScript
grammar_cjkRuby: true
---

# 1、定义类
对象的抽象：类，类必须实例化。
JS实现类的定义：就是一个函数，也是构造函数。
定义类的两种方式：使用prototype原型方式，和使用普通函数方式。JS类是由函数和原型结合组成的。
对象实例化使用new关键字。
JSON是对象的字面量形式。
# 2、创建对象（实例化）的四种方式：
传值---传入属性值
默认值---定义默认值
动态添加--外部动态添加属性
混合--以上结合
instanceof的使用，是否是一个类的实例。
# 3、构造函数和普通函数
构造函数如果return一个对象，new 的时候返回的是return的对象。
# 4、属性
万物都是属性，方法也可以看做属性。
万物都是变量，方法也可以是变量。

函数声明和函数表达式的（变量的形式）不同：函数表达式不能提前访问（不能提升），必须先定义后使用。函数声明可以先使用在声明（只是顺序上）。

Object.defineProperty(obj, prop, descriptor)方法：直接在一个对象上定义一个属性。
三个参数为：定义属性的对象，定义的属性，描述符
重点在描述符的定义，描述符也是一个对象，通常用json对象。

描述符可以定义：属性包装器和权限。
属性包装器：get和set取值器。
公有/私有属性：外部可以访问为公有，外部不能访问为私有。

数据类型检测：
* typeof可以检测的数据有boolean，string，number。不能检测数组，对象，他们都是object；
* 通过call检测数据类型，tostring.call(数据)；----通用
* instanceof检测对象类型的类型。判断数据是哪一个对象的实例。 
* 根据对象的constructor判断类型。通过构造函数，判断是哪个数据对象构造出来的对象。
* jq提供了类型检测。
# 5、实例构造函数。
实例化的过程是：拷贝构造函数属性的过程。还会自动生成一个constructor属性，用来标识是拷贝的哪一个构造函数创建的实例。
# 6、原型（属性搜索机制）
函数对象：Function
函数也是一个对象，constructor是Function的一个属性。prototype也是函数的一个属性。
对象是通过函数实现的。实例化就是对Function对象的拷贝，实例包含函数所有属性和方法。
所以，所有对象都有constructor和prototype属性。

原型对象：prototype指向的对象。
属性方法被共享。(值类型不共享？)
不管实例化多少次，原型只有一个。
修改原型影响所有实例。
实例化只会拷贝构造函数的属性，而不会拷贝原型对象属性，原型使用引用关联的。

即实例包含原型的方式是使用指针指向，而包含函数对象是直接拷贝一份，然后让constructor指向拷贝的对象（就是构造函数）。

双对象法则：
对象是构造函数和原型组合成的。
V8引擎通过__proto__属性连接在一起。构造函数对象中的__proto__指向原型对象。

实例化的对象搜索方法和属性，首先在自己的属性中找，然后从原型（prototype）中找。
属性屏蔽：构造函数属性可以屏蔽原型属性。要使用屏蔽的原型属性，可以delete掉构造函数的属性，或者使用prototype全称访问。

最终原型链：
首先，双对象法则，一切对象都是由原型和函数两个对象组成。

构造函数的__proto__指向原型，也就是__proto__==构造函数.prototype。
实例拷贝构造函数中所有属性，实例拷贝了__proto__==构造函数.prototype。所以实例的__proto__也指向构造函数.prototype。
所以说一个实例是哪一个对象的实例，就是该实例的__proto__指向了该构造函数的.prototype对象。

Object对象是函数对象的实例。
所以object.__proto__==Function.prototype。
Object对象本身的prototype指向为空，即Object.prototype.__protp__==null。
函数对象的原型指向是object的原型，Function.prototype.__proto__==object.prototype。

原型链例子
实例arr
arr.__proto__=== Array.prototype

Array.prototype.__proto__===Function.prototype

Function.prototype.__proto__==object.prototype

Object.prototype.__proto__===null

原型实现继承：
1、原型--将父类的实例作为子类的原型
2、构造--使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
3、实例--为父类实例添加新特性，作为子类实例返回
4、拷贝--拷贝父类实例，附加到子类原型
5、组合--通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用（原型+构造）
6、寄生组合--通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点

重载实现：
arguments+typeof实现重载，判断参数

# 7、函数对象属性----Call法则
函数对象主要属性：
lenght
caller:获取调用当前函数的函数。被谁调用。
callee:返回正在执行的函数对象。正在调用谁。返回自身。 他是arguments的一个属性。
以上的不重要，下面的重要
arguments：函数的参数数组。
constructor：指向构造函数。
prototype：原型对象，可以让它指向一个空对对象，自己扩展。继承中用。

apply
call
bind
tostring

## call法则---借用别人的方法，更改this指向自身。
要借用的方法.call（谁借用，借用方法的参数），要借用方法的this指向会指向谁借用中这个对象。
如果只是借用方法，第一个参数可以设置为null，这样this就不会改变。

伪数组，{}，key为1,2,3,4....的json对象。必须要有一个length属性，不能使用数组内置函数。
真数组，[]，自带length属性，自带很多内置方法。
伪数组转化真数组：
Array.prototype.slice.call(伪数组);
jq是伪数组实现，使用了大量JSON对象。
Apply和call一样，就是call参数是平铺的，apply参数是一个数组。

# 8、拷贝对象和扩展对象
通常使用extend扩展第三方框架中的配置对象，构成自定义配置。
# 9、BOM对象
## window对象
doucument对象
frames对象
history对象
localtion对象
navigator对象
screen对象
## windows属性
parent----如果当前为framens，指向包含当前framens的framens
self----当前窗口
top---指向包含该framens的top-level的window对象。
window---但前window
opener---打开窗口者
## 打开新窗口和控制窗口和滚动
open()
close()
moveBy(x,y)
moveTo(x,y)
x,y以屏幕左上角为标准。
resizeBy(w,h)
resizeTo(w,h)

scrollTo(x,y)
scrollBy(x,y)
## 对话框方法
alert()
comfirm()
prompt()
## 焦点
 focus()
 blur()
## 时间间隔
setTimeout()
clearTimeout()
setInterval()
clearInterval()
## 地址栏对象locationAPI
hash
host--------主机名和端口号
hostname----主机名
herf----------url
pathname-----url路径部分
port-------------端口
protocol-----url中的协议
seach------ur中的参数

## 历史对象historyAPI
length---历史数量
go(-1)
.back()
.forward()
## navigatorAPI
appCodeName
appName
appVersion
userAgent
platform----win32,win16,wince,mac68k,mappc,hp-ux,sunos
cpuClass
online
cookieEnabled
## 浏览器特征
ie：mozilla/x.0，包含mesie x.0
firefox：mozilla/x.0，包含windows nt和firefox/
chrome：mozilla/x.0，包含windows nt和chrome/
opera:opera/，包含windows nt和presto
## eval()方法
## parseInt()	parsefloat()
# 10、DOM对象
DOM树
节点：
parentNode--父节点
childNode--子节点
firstChild----第一个子节点
lastChild----最后一个子节点
nextSibling----下一个兄弟节点
previoursSibling---上一个兄弟节点
元素节点：
firstElementchild---第一个子元素
lastElementChild---最后一个子元素
nextElementSibling---下一个兄弟元素
previousElementSibling---上一个兄弟元素
childElementCount---子元素节点数
# 11、jquery
## jq选择器
element
#id
.class
## jq层级选择器
partent>child
prev + next
prev ~ siblings
## jq基本过滤器查询
## jq内容过滤器
:contains(text)
:empty
:hsa(selecter)
## jq可见过滤器
:hidden
:visible
## jq元素属性--attr
## jq元素内容--html()
## jq元素值--val()
## jq样式css--css()
## jq其他
hasClass
addClass
removeClass
height
width
offset
position

# 13、框架，污染 
  ## 框架通常包含
  1、事件
  绑定事件，接触绑定，点击事件，鼠标事件，键盘事件
  2、选择框架 
  id，class，tag，层次，多组，，层次+多组，html5...
  3、CSS框架
  属性 ，内容，宽，高，偏移，定位，添加class，移除class...
  4、动画
  单物体，多物体...
  ## 污染的决解方式
  1、函数
  2、对象
  3、命名空间---代码分类管理。
  4、立即函数---暴露立即函数变量的方法。
 
  ## 链式访问原理
  重点：返回this；
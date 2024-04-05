
[TOc]
## 小程序与普通网页开发的区别

- 网页开发渲染线程和脚本线程是互斥的，
    这也是为什么长时间的脚本运行可能会导致页面失去响应。
  小程序中，二者是分开的，运行在不同的线程中。
- 网页开发者可以使用到各种浏览器暴露出来的 DOM API，进行 DOM 选中和操作。

DOM："文档对象模型"（Document Object Model）
DOM 是一个编程接口，提供了在编程语言中访问和操作网页内容的能力。
它将网页视为一个由节点和对象组成的树形结构，
从而使得开发者可以改变文档的结构、样式和内容。

## 小程序代码组成
小程序由配置代码JSON文件、模板代码 WXML 文件、样式代码 WXSS文件以及逻辑代码 JavaScript文件组成

### JSON是一种数据格
JSON文件在小程序代码中扮演静态配置的作用， 无法动态更新
JSON 中不能使用注释
app.json

文件格式
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "key":8
}
JSON 的 key 必须包裹在一个双引号里。
JSON 的值只能是： 
1,数字，2,字符串， 3,Bool 值：true 或者 false， 4,数组， 5,对象， 6,Null


### WXML 模板
WXML 全称是 WeiXin Markup Language， 是小程序框架设计的一套标签语言，结合小程序的基础组件、事件系统，可以构建出页面的结构。

#### WXML 介绍
不带逻辑功能的 WXML 基本语法
<标签名 属性名1="属性值1" 属性名2="属性值2" ...> ...</标签名>
​一个完整的 WXML语句由一段开始标签和一段结束标签组成，在标签中可以是内容，也可以是其他的 WXML 语句，这一点上同 HTML 是一致的。
WXML 和 HTML 不同的地方：
1，WXML 要求标签必须是严格闭合的，没有闭合将会导致编译错误。
2，WXML中的属性是大小写敏感的

#### 数据绑定
在 Web 开发中，开发者使用 JavaScript 通过Dom 接口来完成界面的实时更新。
在小程序中，使用 WXML 语言所提供的数据绑定功能，来完成此项功能。

属性值也可以动态的去改变，有所不同的是，属性值必须被包裹在双引号中。

#### 逻辑语法，条件逻辑，列表渲染 模板 引用
WXML 提供两种文件引用方式import和include
import 可以在该文件中使用目标文件定义的 template
include 可以将目标文件中除了 <template/> <wxs/> 外的整个代码引入，相当于是拷贝到 include 位置
#### 共同属性， 就是属性
id, 组件的唯一标识。整个页面唯一
class, 组件的样式类。在对应的 WXSS 中定义的样式类
styel, 组件的内联样式。可以动态设置的内联样式
hidden, Boolean, 组件是否显示。 默认显示
data-*, 自定义属性。组件上触发事件时，会发送给事件处理函数
bind*/catch*, 类型为 EventHandler， 组件的事件


### WXSS 样式
WXSS（WeiXin Style Sheets）是一套用于小程序的样式语言，用于描述WXML的组件样式，也就是视觉上的效果

项目中的样式有几种情况
项目根目录里的项目公共样式
页面样式
其他样式： 可以被项目公共样式和页面上述引用

#### 尺寸单位
在WXSS中，引入了rpx（responsive pixel）尺寸单位。
目的：为了适配不同宽度的屏幕，开发起来更简单

#### WXSS引用
* CSS
引用格式：@import url('./test_0.css')
CSS 里不会把 test_0.css 合并到 index.css 里，
而是请求 index.css 的时候，会多一个 test_0.css 的请求
![alt text](image-12.png)

* WXSS
WXSS 最终会被编译打包到目标文件中，用户只需要下载一次。
在使用过程中不会因为样式的引用而产生多余的文件请求。

#### 内联样式
https://developers.weixin.qq.com/ebook?action=get_post_info&docid=000c44c49141887b00864fbba5100a

#### 选择器
小程序WXSS支持的选择器
![alt text](image-13.png)
优先级权重
![alt text](image-14.png)

## JavaScript 脚本
ECMAScript是一种由Ecma国际通过ECMA-262标准化的脚本程序设计语言， JavaScript 是 ECMAScript 的一种实现。
小程序中的 JavaScript 同浏览器以及NodeJS有所不同后。

ECMA-262 规定了 ECMAScript 语言的几个重要组成部分：
语法
类型
语句
关键字
操作符
对象

浏览器中JavaScript：
ECMAScript  DOM  BOM
DOM（文档对象模型）
BOM（浏览器对象模型）
Web前端开发者利用这两个对象模型去操作浏览器的一些表现，比如修改URL、修改页面呈现、记录数据
![浏览器中JavaScript](image-15.png)

NodeJS中JavaScrip：
ECMAScript  NPM  Native
NPM: 包管理系统，通过各种拓展包来快速的实现一些功能，
同时通过使用一些原生的模块例如 FS、HTTP、OS等来拥有一些语言本身所不具有的能力。
![NodeJS中JavaScrip](image-16.png)

小程序中 JavaScript：
ECMAScript  小程序框架  小程序 API
因为没有 BOM 以及 DOM， 所以类似JQuery、Zepto这种浏览器类库是无法在小程序中运行起来的。
因为没有 Native 模块和 NPM 包管理的机制，小程序中无法加载原生库，也无法直接使用大部分的 NPM 包。

#### 小程序的执行环境
不同的平台的小程序的脚本执行环境也是有所区别的。
小程序的三大运行平台：
iOS平台，包括iOS9、iOS10、iOS11
Android平台
小程序IDE
每个平台使用的 ECMAScript 标准不同，不同标准中定义的语法和关键字是有所不同的，这样有些代码在有些平台上会出现语法错误。

#### 模块化
浏览器中，所有 JavaScript 是在运行在同一个作用域下的，定义的参数或者方法可以被后续加载的脚本访问或者改写。
小程序中，任何一个JavaScript 文件可以作为一个模块，通过module.exports 或者 exports 对外暴露接口

#### 脚本的执行顺序
浏览器中，脚本严格按照加载的顺序执行
小程序中，
小程序的执行的入口文件是 app.js 。并且会根据其中 require 的模块顺序决定文件的运行顺序。
当 app.js 执行结束后，小程序会按照开发者在 app.json 中定义的 pages 的顺序，逐一执行。

#### 作用域
在文件中声明的变量和函数只在该文件中有效。
当需要使用全局变量的时，通过使用全局函数 getApp() 获取全局的实例。

- 定义全局变量
```
// app.js
App({
  globalData: 1
})
```

- 添加全局变量
```
// a.js
// 获取全局变量
var global = getApp()
global.globalValue = 'globalValue'
```

## 小程序宿主环境
微信客户端给小程序提供了宿主环境。
优点：小程序可以调用宿主环境提供的微信客户端的能力，使得小程序比普通网页拥有更多的能力。
缺点：小程序对于不同版本的宿主环境，需要考虑做程序上的兼容处理。

小程序的运行
小程序的运行环境分为渲染层和逻辑层
WXML 模板和 WXSS 样式工作在渲染层
JS 脚本工作在逻辑层

### 渲染 "hello world" 页面
通过例子看小程序如何把脚本里的数据渲染到界面上
WXML 模版使用 view 标签， 利用 {{}} 语法绑定一个 msg 的变量
```
<view>{{ msg }}</view>
```
js 脚本里使用 this.setData 方法把 msg 字段设置成 "hello world"
```
Page({
  onLoad: function () {
    this.setData({ msg: 'Hello World' })
  }
})
```
从上面的例子里可以看到 3 点：
1， 渲染层和数据相关。
2， 逻辑层负责产生、处理数据。
3， 逻辑层通过 Page 实例的 setData 方法传递数据到渲染层

第 1 点涉及了 “数据驱动” 的概念。
第 3 点涉及了 “通信模型”


#### 通信模型
渲染层和逻辑层分别由两个线程管理：
渲染层的界面使用了 WebView 进行渲染；多个界面就需要多个 WebView 线程。
逻辑层采用了 JsCore 线程运行 JS 脚本。

这两个线程的通信会经由微信客户端(用 Native 表示) 做中转；
逻辑层发网络请求也经由 Native 转发。

![渲染层和逻辑层通信模型](image-17.png)

#### 数据驱动
如何让界面视图和变量状态相关联，让他们绑定在一起。
![WXML结构和JS对象均可以表示一棵Dom树](image-18.png)
WXML可以先转成JS对象，然后再渲染出真正的Dom树

当变量状态改变了，
产生的 JS 对象对应的节点就会发生变化。
对比前后两个 JS 对象得到变化的部分，
然后把这个差异应用到原来的 DOM 树上， 从而达到更新 UI 的目的，
这就是数据驱动的原理

对比 JS 对象数据变化， 更新视图层的 DOM 树

#### 界面渲染
在渲染层，宿主环境会把WXML转化成对应的JS对象
在逻辑层发生数据变更的时候，我们需要通过宿主环境提供的setData方法把数据从逻辑层传递到渲染层，
再经过对比前后差异，把差异应用在原来的Dom树上，渲染出正确的UI界面

![逻辑层传递数据到渲染层](image-19.png)


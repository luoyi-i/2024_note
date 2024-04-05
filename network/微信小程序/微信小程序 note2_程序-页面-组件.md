[TOC]

## 程序与页面
### 程序
- 程序构造器 App()
宿主环境提供了 App() 构造器来注册一个程序 App
    - 必须写在项目根目录的 app.js 里
    - App 实例是单例对象，在其他 JS 脚本中用 getApp() 来获取程序实例
    - App() 接受一个 Object 函数， 里面包括回调函数和全局变量
```
App({
  onLaunch: function(options) {},
  onShow: function(options) {},
  onHide: function() {},
  onError: function(msg) {},
  globalData: 'I am global data'
})
```
- 全局数据
所有页面的脚本逻辑都跑在同一个 JsCore 线程，
页面使用的定时器在跳转到其它页面前需要开发者清理

### 页面
- 页面组成和路径
包括
  * 界面， WXML， WXSS
  * 配置， JSON
  * 逻辑， JS 
一个页面的文件需要放置在同一个目录下，必须存在 WXML, JS
页面路径要在 app.json 中的 pages 字段声明，才能被注册到宿主环境中。

- 页面构造器 Page()
宿主环境提供了 Page() 构造器用来注册一个小程序页面
Page() 在页面脚本 page.js 中调用， 接受一个 object 函数， 包括可 data 属性用于设置页面的初始数据和回调函数

- 页面的生命周期和大哭函数
----- 当前项目不需要用到复杂的功能

### API 
wx 对象是小程序的宿主环境所提供的全局对象.
所有小程序的 API 都挂载在 wx 对象底下（除了Page/App等特殊的构造器）
一般约定
   - wx.on* 开头的 API 是监听某个事件发生的API接口，接受一个 Callback 函数作为参数。当该事件触发时，会调用 Callback 函数。
   - 如未特殊约定，多数 API 接口为异步接口 ，都接受一个Object作为参数。


## 事件
事件包括用户在渲染层上的行为反馈以及组件的部分状态反馈，
这种反馈会通知到逻辑层做相应处理
https://developers.weixin.qq.com/ebook?action=get_post_info&docid=000846df9a03909b0086a50025180a

eg
```
<!-- page.wxml -->
<view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>
```

事件通过 bindtap 绑定在组件上的，
同时在 Page 构造器中定义对应的事件处理函数，
当用户点击改 view 区域时达到触发条件生成事件 tap，
该时间处理函数会被执行，同时还会收到一个事件对象 event

4.1 开发流程
在启动开发前，首先我们对整个小程序整体的产品体验有一个清晰的规划和定义，一般会通过交互图或者手稿描绘小程序的界面交互和界面之间的跳转关系。
紧接着，我们优先完成WXML+WXSS还原设计稿，把界面涉及到的元素和视觉细节先调试完成。最后我们把按照页面交互梳理出每个页面的data部分，填充WXML的模板语法，还有完成JS逻辑部分。
当然并不是要完全按照这样的开发流程来开发小程序，有些时候我们可能在产品交互体验还不明确的情况下，先完成JS逻辑层一些模块的工作并做好测试。

4.4 发起 https 网络通信
小程序经常需要往服务器传递数据或者从服务器拉取信息，
这个时候可以使用wx.request这个API。

```
wx.request({

  url: 'https://test.com/getinfo',

  success: function(res) {

    console.log(res)// 服务器回包信息

  }

})
```

向服务器发送请求有两种方法
get， 发送给服务器的数据在 url 里
post， 发送给服务器的数据在 wx.request 的 data 参数里
url 长度有限制，所以一般用 post 方法
传送复杂数据时用 JSON 格式更合适， header 要做设置
header: { 'content-type': 'application/json'},


* 服务器处理请求并饭后 http 包。
小程序端收到回包后会触发 success 回调，回调参数会带上一个 Object 信息。
只要服务器返回就会进入 success 回调，开发者要自己对回包的返回码进行判断再处理。

* 设置超时时间 3s
```
  "networkTimeout": {

    "request": 3000

  }
```
* 请求前后的状态处理
一般的场景：
用户点击一个按钮，
界面出现 “ 加载中... " 的 loading 界面，然后发送一个请求到后台。
后台返回成功直接进入下一个业务逻辑处理，
后台返回失败或者网络异常则显示一个 "系统错误” 的 toast， 
同时一开始的 loading 界面会消失。



当前程序上用的 	
picker	从底部弹起的滚动选择器

## WeUI 组件库
微信官方的 WeUI 组件库 -- weui.wxss。
UI 组件库。

在 wxss 文件开头引入库文件
@import "../../lib/weui.wxss";

## component
https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/lifetimes.html

## 组件生命周期
干嘛不直接叫函数
就是指组件自身的一些函数，在特殊的时间点或遇到特殊的框架事件时被自动触发
最重要的几个函数
created： 可以用于给组件 this 添加自定义属性，但不能调用 setData
attached: this.data 已经被初始化。这个用的最多，可以在这个函数做初始化的工作
detached: 退出页面时，如果组件在还页面节点树种，则 detached 被触发
```
  lifetimes: {
    attached: function() {
      // 在组件实例进入页面节点树时执行
    },
    detached: function() {
      // 在组件实例被从页面节点树移除时执行
    },
  },
```

##  自定义组件
https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/
一个自定义组件由 json wxml wxss js 4个文件组成
在 .json 里声明
```
{
  "component": true
}
```
在组件wxss中不应使用ID选择器、属性选择器和标签名选择器。

### 利用 component 定义组件
https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html
```
Component({
  properties: {
    // 这里定义了innerText属性，属性值可以在组件使用时指定
    innerText: {
      type: String,
      value: 'default value',
    }
  },
  data: {
    // 这里是一些组件内部数据
    someData: {}
  },
  methods: {
    // 这里是一个自定义方法
    customMethod: function(){}
  }
})
```
### 使用自定义组件
```
{
  "usingComponents": {
    "component-tag-name": "path/to/the/custom/component"
  }
}
```
这样，在页面的 wxml 中就可以像使用基础组件一样使用自定义组件。节点名即自定义组件的标签名，节点属性即传递给组件的属性值。


* 注意事项
出于性能考虑，使用 usingComponents 时， setData 内容不会被直接深复制，即 this.setData({ field: obj }) 后 this.data.field === obj 。（深复制会在这个值被组件间传递时发生。）


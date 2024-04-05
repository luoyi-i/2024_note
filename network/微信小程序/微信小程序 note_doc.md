微信小程序

https://bemfa.blog.csdn.net/article/details/107019002?utm_relevant_index=2
wx9fdc0257ee51b910

- 微信公众平台注册小程序，从左侧菜单 "开发" --选择开发管理， 进入界面后选 "开发设置" 拿到小程序 appid
- 下载安装微信开发者工具
- 在微信开发者工具里， 从左侧菜单 "小程序" 进入界面后选导入项目


## 文档

_____ 开发微信小程序
_____ 文档
小程序开发文档
https://developers.weixin.qq.com/miniprogram/dev/framework/
小程序开发指南
https://developers.weixin.qq.com/ebook?action=get_post_info


官网上的视频，分辨率太低，看着费劲
https://developers.weixin.qq.com/community/business/course/000264e20a0dd8e69669b609451c0d

AppID 用测试号， 不使用模版

- 用微信开发者工具开发
- 用可用的模板或已经存在的项目
- 主要三种类型的文件
    * wxml， 这是会直接显示在屏幕上的内容
    * wxss， 这是装饰文件，用来定向屏幕上内容显示的格式
    * js， 这个文件更灵活的定义了内容的显示，比如按键操作，比如定义变量


_____ 名片小程序跨终端升级
![alt text](image-7.png)
![alt text](image-8.png)

#### 腾讯课堂
#### 小程序跨终端升级
_____ 使用 UniApp 
uni-app 是一个使用 Vue.js 开发所有前端应用的框架,
可以把一套代码编到十几个平台

开发工具是 HBuilerX

_____ nodejs
nodejs 是 js 的运行环境
_____ Vue
Vue 是一个 js 框架用于编译用户界面。
基于标准 HTML, CSS, CSS and JavaScript,
提供了声明渲染和基于的编程模型，
帮助开发用户界面
核心特征
* 声明渲染： 基于 js 状态描述 HTML 输出
* 及时反应： 自动跟踪 js 状态变化，如有变好，更新 DOM 

eg：
```
import { createApp, ref } from 'vue'

createApp({
  setup() {
    return {
      count: ref(0)
    }
  }
}).mount('#app')
```
```
<div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
```
输出
![alt text](image-9.png)


实现 
https://www.auteltech.cn/service/4004.jhtml



## hello world
安装微信开发者工具
新建小程序项目，不启用模版
![alt text](image-10.png)

![alt text](image-11.png)

最简代码
app.json
```
{
  "pages": ["pages/index/index"]
}
```
在根目录下新建pages目录，然后在pages目录下新建index目录，接着在index目录下创建两个文件index.wxml和index.js。
index.wxml的内容如下所示。<text>Hello World</text>
index.js的内容如下所示。Page({})

微信开发者工具的模拟器界面上显示出Hello World


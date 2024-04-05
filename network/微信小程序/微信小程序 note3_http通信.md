
## 网络通信

https://developers.weixin.qq.com/ebook?action=get_post_info&docid=000ee27c9c8d98ab0086788fa5b00a

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

### wx.request详细参数
具体见 
https://developers.weixin.qq.com/ebook?action=get_post_info&docid=000ee27c9c8d98ab0086788fa5b00a
|参数名|类型|描述|
|----|----|----|
|url|	String|	服务器接口, 只有这一项需要必填|
|data|Object/String|请求的参数|
|header|object|设置请求的 header， 不能设置 referer， 默认 header header['content-type'] = 'application/json'|
|method|String|默认 GET, 需大写，包括， OPTIONS, HEAD, POST, PUT, DELETE, TRACE, CONNECT|
|dataType|String|回包的内容格式，如果设为 json（默认格式），会尝试对返回的数据做一次 JSON 解析|
|success|Function|收到开发者服务成功返回的回调函数，其参数是一个 Object|
|fail|Function|接口调用失败的回调函数|
|complete|Function|接口调用结束的回调函数|

#### get 方式下传递参数，可以通过url上的参数以及通过data参数
url是有长度限制的，其最大长度是1024字节
``` cpp
// 通过url参数传递数据

wx.request({
  url:'https://test.com/getinfo?id=1&version=1.0.0',
  success: function(res) {
    console.log(res)// 服务器回包信息
  }
})

   // 通过data参数传递数据
wx.request({
  url: 'https://test.com/getinfo',
     data: { id:1, version:'1.0.0' },
  success: function(res) {
    console.log(res)// 服务器回包信息
  }
})

```
#### 复杂的 JSON 格式或大长度的参数更适合用 POST 方式


#### 回包
|参数名|类型|描述|
|----|----|----|
|data|Object/String|开发者返回的数据|
|statusCode|Number|http 状态码|
|hearder|Object|http response header|

请求巴法云 http 设备云关闭 led 返回的数据
```
{data: {…}, header: {…}, statusCode: 200, cookies: Array(0), accelerateType: "none", …}
accelerateType: "none"
cookies: []
data: {code: "40010", status: "sendok"}
errMsg: "request:ok"
header:
Access-Control-Allow-Origin: "*"
Connection: "keep-alive"
Content-Encoding: "gzip"
Content-Type: "text/html; charset=UTF-8"
Date: "Wed, 20 Mar 2024 05:34:47 GMT"
Server: "nginx/1.20.2"
Transfer-Encoding: "chunked"
Vary: "Accept-Encoding"
__proto__: Object
statusCode: 200
__proto__: Object
```


* 服务器处理请求并返回 http 包。
小程序端收到回包后会触发 success 回调，回调参数会带上一个 Object 信息。
只要服务器返回就会进入 success 回调，开发者要自己对回包的返回码进行判断再处理。



####  一般使用技巧

* 在小程序项目根目录里边的app.json可以指定request的超时时间
设置超时时间 3s
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
官方给出来常用例子代码
代码清单4-11 wx.request常见的示例代码
为了防止用户极快速度触发两次tap回调，我们还加了一个hasClick的“锁”，在开始请求前检查是否已经发起过请求，如果没有才发起这次请求，等到请求返回之后再把锁的状态恢复回去。



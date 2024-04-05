express 库
https://gist.github.com/xinyiqy/e3c899e95744e529687eb3d3fe006b16

## express 框架
server.use("/test",function(req,res){
    console.log("-------------------res:", req.query);
    const obj = {
        foo:"hello",
        bar:"world"
    }
    res.end(JSON.stringify(obj));
})

接收的数据在 req 里, reg.query
发送数据在 obj 里

``` js
// server.js
const express = require("express");
const server = express();

// 接收的数据在 req 里, reg.query
// 发送数据在 obj 里

server.use("/test",function(req,res){
    console.log("-------/test------------:req.query", req.query);
    const obj = {
        foo:"hello",
        bar:"world"
    }
    res.end(JSON.stringify(obj));
})

server.use("/search",function(req,res){
    console.log("--------/search-----------:req.query", req.query);
    const obj = {
        result:"succeed",
    }
    res.end(JSON.stringify(obj));
})

server.listen(3000,function(){
    console.log("listening on *:3000");
})

// 根目录的函数定义必须放在最后一个
server.use("/",function(req,res){
    console.log("--------/root-----------:req.query", req.query);
    res.end("<h1>hello world</h1>");
})
```

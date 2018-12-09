# 彩票数据服务器配置

1. 新建项目,进入项目根目录
2. npm init
3. npm i express 
4. 新建 server.js文件,在server.js文件中输入以下内容,就启动了一个简单的服务器
可以通过 http://localhost:8888/来访问
```
var express = require('express');
var app = express();
//
app.get('/',function(req,res){
    console.log();
})

app.listen(8888,function(){
    console.log("服务器已经打开了");
})
```
5. 启动服务器  node server.js

6. 在服务器端解决跨域的问题
```
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Content-Type,Content-Length, Authorization, Accept,X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    if(req.method=="OPTIONS") res.send(200);/*让options请求快速返回*/
    else  next();
});
```

7. 安装 request模块 npm i request
```
通过从node服务访问网易的服务,避免浏览器的跨域保护问题
```
8. 在server.js中引入 request模块
```
const request = require('request');
```
9. 获取自己抓取的数据api,设置基本的api地址
```
const baseUrl = "http://api.caipiao.163.com/missNumber_trend.html?gameEn="
```
10. 自定义api
```
    app.get('/api/ssq',function(req,res){
         //通过request向目的服务器(例如网易)发送请求
         request(baseUrl+'ssq',function(err,response,body){
              //通过res将数据显示到浏览器上
              res.send(body)
         })
    })
```

11. 自定义轮播图服务器
```
1. 在server.js中导入 path模块
var path = require('path');

2.设置服务器静态资源托管
app.use(express.static(path.join(__dirname, 'public')))

可以通过连接直接访问图片
localhost:8888/focus/1533891388968_1.jpg

注意要先在服务上创建public目录,
3.设置轮播图api接口
var imageBase = 'http://localhost:8888/'
app.get('/api/focus',function(req,res){
    var focus = [
        {title:'翻拍赢大礼',imageUrl:imageBase+'/focus/1533891388968_1.jpg'},
        {title:'赚钱三法宝',imageUrl:imageBase+'/focus/1534923480745_1.jpg'},
        {title:'大转盘',imageUrl:imageBase+'/focus/1534993123777_1.jpg'},
        {title:'今日财运走势',imageUrl:imageBase+'/focus/1535614687558_1.jpg'},
        {title:'红包',imageUrl: imageBase+'/focus/1535614872375_1.jpg'}
    ]
    res.send(focus);
})


```

11. 登录验证jwt功能
```
npm i express-jwt body-parser jsonwebtoken 
//为了生成jsonwebtoken
var jwt = require('express-jwt');
var bodyParser = require('body-parser');
为了解决post请求
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

```
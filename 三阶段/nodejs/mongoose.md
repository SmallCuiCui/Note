## MongoDB数据库

是一个非关系型的数据库，数据存储是以类似于json文件格式的文档进行存储，读取到的数据是一个对象

- 本地安装，官网下载4.1.3版本安装(容易成功)，配置环境变量.....MongoDB/server/3.1.4/bin
- 设置数据库存放位置
  - 在任意目录下打开命令行
  - 命令`mongod --dbpath D:/dbFile`  设置数据库存放位置D:/db

* 命令行操作

  3): 再打开一个命令行输入mongo

  4): show dbs: 展示所有数据库

  5): use + 数据库名: 进入某一个数据库，如果没有则会创建一个

  6): show collections: 展示当前数据库所有表单

  7): 查询所有数据: db.表单名.find()

  8): 插入单条数据: db.表单名.save({name: 'xxx', password: '123'})

  9): 插入多条数据: for(var i=200; i< 400; i++) {

  db.student.save({name: "cxk"+i, age: "123"+i})

  }

- Robo 3T可视化的MongoDB数据库管理
- 对照MySQL数据库

| MySQL    | MongoDB    |
| -------- | ---------- |
| database | database   |
| table    | collection |
| col(列)  | files      |
| row(行)  | documents  |

#### 原生用法

- 插入一条  `db.collection.insertOne({})`   collection类似于数据库的表，如users

  或者 db.getCollection('users').insertOne({username:'lisi',password:'123'})

- 插入多条  `db.collection.insertMany([{},{}])`

  db.getCollection('users').insertMany([{},{}])

- 查询 `db.collection.find( {"username":"张三"} )`

  db.getCollection('users').find({})

#### mongoose

基于nodejs来操作mogodb的对象模型

###### 安装

`npm install mongoose`  到根目录下

###### 开启数据库服务

`mongod --dbpath D:/dbFile`  路径为数据库的存放位置

###### 连接数据库

```javascript
//连接本地数据库服务器，连接myDatabase数据库
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/myDatabase',{useNewUrlParser:true});
//得到连接实例，处理连接的情况
var db = mongoose.connection;
//处理连接错误输出
db.on("error",console.error.bind(console,'connection error:'));
//监听一次打开事件
db.once('open',function(){
	console.log("we are connected");
})
```

###### Schema

```javascript
//schema把非关系型数据库装换为关系型结构
var userSchema = new mongoose.Schema({
  name: String,
  password:String
});
//根据userSchema得到一个模型，相当于关系型数据库中的表
//users是数据库下的一个collection，在我这里测试必须加有s，user运行不成功。。
let userModle = mongoose.model("users",userSchema);

```

###### 插入 

实例.save()  使用实例来存

```javascript
//根据users模型得到一个实例，相当于表中的一条数据
let kitty = new userModle({username:"kitty",password："123"})

//存数据，异步方法。将实例存进MongoDB中
kitty.save(function(err,kitty){
	if(err) return console.error(err);
	else console.log("succ);
})
```

###### 查询

 model().find()  使用模型进行查询

```
userModle.find(where,(err,docs)=>{
	if(err) return console.error(err);
	console.log(docs);
})

```

###### 更新

updateOne()与updateMany()

~~~javascript
//@param where<Object> 更新条件
//@param updated<Object> 更新数据
//只更新第一条匹配
userModle.updateOne(where,updated,(err,res)=>{
    if(err)reject(err);
    else resolve(res);
    //res:{ ok: 1, nModified: 1, n: 1 }

})
				
//更新所有匹配
userModle.updateMany(where,updated,(err,res)=>{
    if(err)reject(err);
    else resolve(res);
})
~~~

###### 删除

deleteOne()与deleteMany()

~~~javascript
//@param where<Object> 删除条件
//删除匹配的第一条
userModle.deleteOne(where,err=>{
    if(err) reject(err);
    else resolve()
})
//删除所有匹配			
userModle.deleteMany(where,err=>{
    if(err) reject(err);
    else resolve()
})
~~~

#### robo3T

mongoDB可视化界面

#### MVC

- M：模型层，数据操作，最底层
- V：视图层，
- C：控制层，逻辑层，调用模型层封装的方法

## 接口

前端进行数据请求

#### 接口使用

* 开发环境(代理跨域) 

  前端：http://lcoalhost:3000

  baseUrl = http://www.91.com

  $.get(baseUrl+ '/api/user/get')

  接口：http://www.91.com/api/user/get


* 产品环境

  前端：http://www.91.com

  $.get('/api/user/get')

  接口：http://www.91.com/api/user/get

#### 接口文档

* url:/api/list

* method:get

* query:null

* dataType:json

* data:<Object>

  | 字段名     | 类型   | 示例值      | 说明                     | 是否必选 |
  | ---------- | ------ | ----------- | ------------------------ | -------- |
  | code       | Number | 200         | 200代表成功，201代表失败 | 是       |
  | body       | object |             | 返回的数据               | 是       |
  | \| list    | array  |             | 商品列表数据             | 是       |
  | \|\| id    | number | 1           | 商品的id                 | 是       |
  | \|\| title | string | 薯片-大波浪 | 商品名称                 | 是       |
  |            |        |             |                          |          |

  数据示例

  ~~~javascript
  {
      "code": 200,
    	"body": {
          "list": [
              {
                  "id": 1,
                	"title": "薯片-大波浪"
              },
            	{
                  "id": 2,
                	"title": "薯波浪"
              },
            	{
                  "id": 3,
                	"title": "薯-大浪"
              }
          ]
      }
  }
  ~~~


#### 处理数据接口的路由

* 前端通过接口数据请求

  ~~~javascript
  //resp就是该接口返回的数据
  $.get('/api/shop/list', resp => {
        console.log(resp)
        if (resp.code === 200) {
          resp.body.list.forEach(shop => {
            $('<li>').html(`<span>${shop.id}</span><b>${shop.title}</b>￥<i>${shop.price}</i><a href='javascript:;'>删除</a>`)
              .appendTo($('ul'))
          });
        }
      })
  ~~~

* 后端响应接口请求，并返回数据

  ~~~javascript
  const router = require('express').Router()
  router.get('/shop/list', (req, resp) => {
    // 应该从数据库里拿到商品列表，resp返回给前端
    // 模拟数据列表list
    resp.json({
      code: 200,
      body: {
        list
      }
    })
  })
  ~~~

#### Ajax请求

状态码，

#### 跨域问题

###### jsonp

利用src的开放性原则

###### cors

* 后端使用cors解决跨域-但是一般不轻易允许跨域

  程序前面加入代码：	header("Access-Control-Allow-Origin:*");

* 客户端node处理跨域

  * 安装`npm install cors`  引入：const cors = require('cors')

  * 全部跨域允许

    ~~~javascript
    var express = require('express')
    var cors = require('cors')
    var app = express()
    app.use(cors())
    ~~~

  * 单个路由允许所有跨域

    ~~~javascript
    var router = require('express').Router()
    var cors = require('cors')
    router.get('/shop/list', cors(),(req,resp)=>{
        //来自/shop/list请求
    })
    ~~~

  * 单个路由指定允许来自某个域的请求

    ~~~javascript
    //运行在localhost:8090下的数据接口
    //处理/api的路由
    var router = require('express').Router()
    var cors = require('cors')
    var corsOptions = {
      origin: 'http:localhost',
      optionsSuccessStatus: 200 
    }
    router.get('/shop/list', cors(corsOptions),(req,resp)=>{
        //处理/shop/list请求
    })
    ~~~

    ~~~javascript
    //运行在Apache服务器下(localhost)，跨域请求localhost:8080下的接口
    $.get('http://localhost:8090/api/shop/list', resp => {
          console.log(resp)
    })
    ~~~

###### 代理跨域Proxy

客户端从自己服务器请求接口数据，服务端将接口请求(存在跨域)代理到相应的服务器上，处理后该接口就是本域进行访问(服务器之间不存在跨域)。在获取到该服务器的数据返回后，由自己的服务器再将数据返回给前端。这样对于前端(浏览器)来说就是从自己服务器请求的数据，不存在跨域了。

* 安装中间件`npm install http-proxy-middleware --save`

* 简单使用

  ~~~javascript
  // 引入proxy模块
  var proxy = require('http-proxy-middleware')
  // 基本小demo：配置代理的目标，来自于/api的请求就代理到https://apiv2.pinduoduo.com
  //target参数就是数据接口存在的服务器域名
  app.use('/api', proxy({ target: 'https://apiv2.pinduoduo.com', changeOrigin: true }))
  ~~~

* example-较复杂处理

  ~~~javascript
  var options = {
      //数据接口所在的源域名
    target: 'https://apiv2.pinduoduo.com', // 代理目标源
    changeOrigin: true, // 需要切换源
    ws: true, // 代理长连接
    pathRewrite: {
      // 重写路径：前端的路径可能并不是真实接口的路径，在代理的时候重写回真实路径
      '^/api/goods': '/api/fiora/subject/goods' // rewrite path
    },
    router: {
      // 
      'dev.localhost:3000': 'http://localhost:8000'
    }
  }
  // 根据配置得到proxy的实例
  var goodsProxy = proxy(options)
  // 中间件
  app.use('/api', goodsProxy)
  ~~~

## 登录/注册 

express()下进行登录注册，前端只需要携带参数进行接口的访问，即可获得登录注册的结果。一般不进行前端cookie的存储，不安全。在前端叫cookie，后端叫session

#### cookie-session

后端将用户的登录信息编码之后存储在cookie下。

###### 安装

`npm install cookie-session`

###### 引入

~~~javascript
const cookieSession = require('cookie-session')
~~~

###### 使用中间件

使用之后会在req上增加一个session对象

~~~javascript
// 使用cookieSession中间件
app.use(cookieSession({
  name: 'session',
  keys: ['aabbcc'],
  maxAge: 24 * 60 * 60 * 1000
}))
~~~

###### 登录时存cookie

~~~javascript
//处理前端对登录接口的请求。（用户登录的路由）
router.post('/login',function(req,resp){
    //进行数据库访问，判断是否登录成功
    //登录成功，得到查询的docs的对象数组，然后存cookie
    req.session = {
        isLogin:true,
        user:docs[0]//第1条数据及查询到的用户
    }
}）
~~~

###### 注销登录清除cookie

~~~javascript
router.post('/logout',function(req,resp){
	req.session = null;
})
~~~

###### 判断是否登录

~~~javascript
router.post('/islogin',function(req,resp){
    //直接调用req.session.isLogin方法即可
	if(req.session.isLogin){
		//表示已经登录状态
        //req.session.user.username获取登录的用户信息
        resp.json({
            'res_code':200,
            'res_message':req.session.user.username
        })
	}
})
~~~

#### Crypto加密

crypto属于node的核心对象，使用时直接引入

~~~javascript
const crypto = require('crypto');
//加密，此过程好像不可逆，当要进行密码对比时，以相同的方式加密之后对比即可
password = crypto.createHash('sha224').update(password).digest('hex')

~~~

#### create-hash加密

create-hash属于npm的一个包，使用之前需要npm安装`npm install creat-hash -S`

~~~javascript
const createHash = require('create-hash');
password = createHash('sha224').update(password).digest('hex')
~~~

## mock.js假数据

在本地搭建假数据，它类似于rap2的假数据生成，但是rap2是在线的。本地手动的生成假数据，应用于某个处理前端数据接口的路由下，向前端随机的返回假数据。

### npm安装

* 安装 `npm install mockjs`

### 使用

~~~javascript
//假设是处理前端商品数据请求的路由。shop.js
const router = require('expres').Router()
const Mock = require('mockjs')

router.get('/list',(req,resp)=>{
    const res_body = Mock.mock({
        "list|1-10": [{
            "id|+1": parseInt(Math.random * 1000),
            "title": '@ctitle(20,30)',
            "price": '@Float(100,200.2,2)',
            "img": '@image(300x100, @color, #ff,png,@word)'
        }];
        resp.json({
        res-code：200，
        res_body
    })
    })
})


~~~

## http连接

http实现长连接，socket技术。
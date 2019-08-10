## Express框架

基于node平台，快速，开放，极简的Web开发框架

#### 安装

- 全局安装` npm install express -g`
- `mkdir myapp`新建文件夹，并cd到给文件夹
- `npm init -y`初始化项目的package.json文件
- `npm install express -S`   在myapp文件下安装express并保存到依赖列表

#### hello world

```javascript
//myapp下的main.js文件   运行：node main
const express = require('express');
const app = express();

app.get('/',(req,resp)=>{
	res.send("hello world");
})

app.listen(3000,()=>{
    console.log("app demo 已经监听在3000端口")
})
```

#### express 应用生成器

通过应用生成器工具 `express-generator` 可以快速创建一个应用的骨架。

- `npm install express-generator -g `全局安装生成器工具

- `express -h`查看命令行参数

- 使用express创建应用`express --view=ejs exApp`其中参数ejs是一种模板引擎

- `cd exApp`进入项目

- `npm install`安装依赖，express创建的应用都有一些需要的依赖，如http等

- `port=8090 npm start`启动项目，端口参数可选，不设置`npm start`的话默认为3000端口

  ```javascript
  //package.json文件下的scripts，可在里面自定义一些命令,
  //如上面执行的start命令，就是执行一个www文件
  "scripts": {
      //运行 npm start
      "start": "node ./bin/www"，
      //自定义命令，采用npm run st运行
      "st":"nodemon ./bin/www"
    }
  
  //www文件，创建服务，处理端口，引入app模块
  //app.js文件 引入各类模块，设置模板引擎的文件(view)，使用应用需要的中间件
  //路由文件模块，处理请求，并返回给前端数据
  //public文件夹：放置静态资源的文件夹
  //views文件夹：放置模板的文件夹
  
  
  //项目执行过程
  //执行start命令->本质执行www文件 =>www文件创建了服务并监听 =>前端传来请求 =>从www到引入app模块 =>app模块引入了很多依赖的模块、使用中间件、配置等 =>根据请求url进入对应路由 =>routers下的对应路由模块 =>路由中根据url返回对应资源
  ```

#### express框架运用

###### 路由

确定应用程序如何响应客户机的特定端点请求，该请求可以是一个路径，也可以是一个http请求方法

- app.METHOD(PATH,HANDLER)

```javascript
//使用规则：app.METHOD(PATH,HANDLER)
app.get('/', function (req, res) {
  res.send('Hello World!');
    //res.end()  res.json()  res.render()   res.send()  res.sendFile()
})
app.get('/', function (req, res) {},function(){})
app.get('/', [fun1,fun2,fun3])
```

- app.route()

  为路由路创建处理程序

  ```javascript
  app.route('/book')
    .get(function (req, res) {
      res.send('Get a random book')
    })
    .post(function (req, res) {
      res.send('Add a book')
    })
  ```

- express.Router()

  定义一个路由模块，在app.use()中使用

  ```javascript
  //定义 index.js
  var express = require('express);
  var router = express.router();
  router.use(...);
  router.get("/",function(req,resp){
      res.send("hello");
  })
  //暴露此模块
  module.expres = router;
  ```

  ```javascript
  //加载路由模块到app应用
  var app = express();
  var indexRouter = require("./index");
  
  app.use("/",indexRouter)
  ```

###### 中间件

中间件函数能够访问请求对象，响应对象，以及应用程序的请求/响应循环中的下一个中间件函数。下一个中间件函数使用next()的变量来表示。

中间件的使用顺序会影响页面渲染效果

中间件有应用层中间件，路由层中间件，错误处理中间件，内置中间件，第三方中间件。

```javascript
//应用层
var app = express();
//无挂载，应用程序每收到请求时执行
app.use(function(req,resp,next){
    console.log("111");
    //当前中间件函数没有结束请求响应周期，则必须调用Next（）将控制权传递给下一个中间件函数
    next();
});
app.use("/",function(req,resp,next){});//在/路径中为任何类型的 HTTP 请求执行此函数。
app.get("/",function(req,resp,next){});//处理针对 / 路径的 GET 请求。

//路由层
var router = express.Router();
router.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});
app.use('/', router);

//错误处理中间件，处理函数有四个变量
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

###### 静态资源

提供图片，js，HTML，css等静态资源，使用Express 中的 `express.static` 内置中间件函数

```javascript
// 处理静态文件的中间件（静态资源从public中获取)
// 此处访问静态资源的时候需要将资源的文件后缀带上，不然应该会识别成路径进行路由分配
app.use(express.static(path.join(__dirname, 'public')));

// 此静态资源可通过带有/routes前缀来进行访问
app.use('/routes',express.static(path.join(__dirname, 'routes')));
```

###### multer-文件上传

处理multipart/form-data类型的表单数据，主要用于文件上传

安装`npm install multer --save`  

会添加`body` 对象以及`file`或`files`对象到express的request对象中，body包含表单文本域信息，file/files包含对象表单上传的文件信息

```javascript
const express =require('express');
const app = express();
const fs = require('fs');
var bodyParser = require('body-parser');// 编码解析
var multer = require('multer');

// 配置静态文件的获取位置
app.use('/public',express.static('public'));
// 创建编码解析，post方法需要
app.use(bodyParser.urlencoded({extended:false}));
// dest参数设置文件保存的位置，multer()后面的函数array()设置语序上传的文件
app.use(multer({dest:'/static/'}).array('image'));

app.get('/form.html',function(req,resp){
	resp.sendFile(__dirname+"/public/form.html");
})

app.post('/file_upload',function(req,resp){
	// 上传的文件信息，此files是multer添加到请求对象上的，包含对象表单上传的文件信息。
	console.log(req.files[0]);
	/*{ fieldname: 'image',
	  originalname: '1.jpg',
	  encoding: '7bit',
	  mimetype: 'image/jpeg',
	  destination: '/static/',
	  filename: '470e018728f94659e4c4a8e4dd41ac7b',
	  path: '\\static\\470e018728f94659e4c4a8e4dd41ac7b',
	  size: 25117 }*/

// 拼接保存路径
	var des_file = __dirname + '/static/' + req.files[0].originalname;
	fs.readFile( req.files[0].path,function(err,data){
		
		fs.writeFile(des_file, data, function(err){
			if(err){
				console.log( err );
			}else{
				response = {
					message:'File uploaded successfully',
					filename:req.files[0].originalname
				}
			}
			resp.end( JSON.stringify( response ));
		})
	})
})

var server = app.listen(8081)
```

```html
<!--表单内容-->
<form action="/file_upload" method="post" enctype="multipart/form-data">
    <p><input type="file" name="image" size="50"></p>
    <input type="submit" value="上传文件" name="">
</form>
```

#### 常用API

###### express()

- express.json()  express内置中间件，处理json格式数据的中间件

- express.static(root)  root参数指定为静态资源提供服务的根目录

  ```javascript
  //设置静态资源的获取在当前目录找过去的public文件夹下
  app.use(express.static(path.join(__dirname, 'public')));
  ```

- express.router()  创建新的router对象

- express.urlencoded() 解析url的中间件

###### application

- app.all(path,callback[,callback])  类似app.METHOD()方法，但它匹配所有http请求

- app.lesten(port) 将连接绑定并监听到特定端口

- app.METHOD()  路由HTTP请求，其中method是请求http的方法，如get，post，put等

- app.get(path,callback[,callback]) 使用指定的回调函数将HTTP GET请求路由到指定的路径

- app.post(path,callback[,callback..])使用指定的回调函数将HTTP POST请求路由到指定的路径

- app.put(path,..)使用指定的回调函数将HTTP PUT请求路由到指定的路径

- app.render() 返回根据回调函数渲染的HTML，类似于resp.render(),但它不能将结果返回给客户端

- app.route() 返回一个路由实例

- app.set(name,value)  name值可以是配置服务器的行为的特殊名称值，如views,视图文件的目录

  ```javascript
  app.set('views', path.join(__dirname, 'views'));
  // 设置模板引擎为ejs
  app.set('view engine', 'ejs');
  ```

- app.use([path,]callback) 指定一个或多个中间件到指定路径，当请求路径匹配时将执行中间件函数，未指定路径时将响应所有路径请求。

- app.param() 向路由参数添加回调触发器

###### request	

- req.url  获取请求路径，根据路径匹配对应路由

###### response

- resp.end([data],[,encoding])  结束响应进程

- resp.download() 

- resp.get()

- resp.json()

- resp.render() 渲染模板HTML并将结果发送到客户端

  ```javascript
  /* GET home page. */
  router.get('/', function(req, resp, next) {
  	// 此处的index是view下的index.ejs文件，在app.js中设置过此路径
  	//app.set('views', path.join(__dirname, 'views'));
  	// 渲染index模板引擎，并且将渲染结果返回给前端。区别于之前的前端渲染方法
    resp.render('index', { title: 'nihao' });
  });
  ```

- resp.send() 传送http响应内容，可以是一个字符串，对象，数组

- resp.sendFile(path,[,callback])  除非在选项对象中设置了根选项，否则path必须是文件的绝对路径。

  ```javascript
  let filePath = path.join(__dirname,'../public/list.html');
  resp.sendFile(filePath);
  ```

- resp.status(code) 设置http状态的处理

  ```javascript
  res.status(400).send('Bad Request');
  ```

###### router

- router.all()
- router.METHOD()
- router.param()
- router.route()
- router.use()
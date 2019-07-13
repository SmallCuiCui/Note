## node相关

#### nvm管理node

###### nvm

node版本管理器：安装管理node，用于多个node版本的切换。

###### 下载安装

github搜索nvm，下载安装，注意路径中不要出现中文及空格

###### 常用命令

* `nvm -v`  查看nvm版本，以及nvm命令

* `nvm ls `   查看当前已经安装的node版本
* `nvm install 10.15.3`   安装指定node版本,默认安装64位，安装32位`nvm install 10.15.3 32`
* `nvm uninstall  10.15.0`  卸载指定版本node
* `nvm  use 10.15.3`  使用指定node版本
* `nvm  root`   查看node版本存储的目录
* `nvm  root [path]`   设置nvm 储存node版本的位置，如果未设置，使用当前路径

#### sublime配置node

* 使用install pakeage 安装nodeij插件

* 配置Nodejs.sublime-settings文件

  Preferences-->Package Setting-->Nodejs-->Settings-User

  把 node_command 和  npm_command 分别配置成安装node.exe与npm.cmd文件的绝对路径

  ~~~
  "node_command": "G:\\app\\node.js\\install\\node.exe",
  
  "npm_command": "G:\\app\\node.js\\install\\npm.cmd"
  ~~~

* 配置Nodejs.sublime-build文件

  Preferences-->Browse Package ->Package文件夹->Nodejs文件夹->Nodejs.sublime-settings文件

  修改cmd 和 windows 下的cmd 以及encoding

  ~~~
  "cmd": ["G:\\app\\node.js\\install\\node.exe", "$file"],
  ....
  "encoding": "utf8",
  .....
  "windows":
      {
          "cmd": ["G:\\app\\node.js\\install\\node.exe","$file"]
      },
  ~~~

* 采用nvm控制node版本时，node与npm的绝对路径写nvm下system中的nvm与node路径

#### npm

JavaScript包管理工具

* `npm root -g`查看npm全局包安装位置
* `npm config set prefix  ""`  设置npm全局包安装的位置(node_modules文件夹)，然后需要将该路径添加到环境变量中
* `npm install xxx ` 安装依赖包
* `npm init -y`  初始化一个package.json文件，加上-y就会默认生成该文件

#### CommonJS规范

nodejs的规范，之前使用的require满足AMD规范

* 模块化，require下只有满足AMD规范的文件才能使用require引入，同样的，nodejs下满足commonjs的才能require引入。自己编写的js模块就需要满足commonjs规范。

* 使用npm安装的依赖包即采用require引入，如前面用到的gulp工具即是；

###### 模块化

* commanjs模块规范主要分为：模块定义，模块标识，模块引用
* 一个文件就是一个模块，拥有单独的作用域
* 普通方式定义的函数，参数，对象都属于该模块内
* 通过exports与module.exports来暴露模块中的内容
* 通过require加载该模块，多个模块同步加载。加载时如果为绝对路径则直接根据路径加载，最快，不是路径则到node_modules文件下遍历查找。

* 代码运行在该模块内，不会污染全局，模块可以多次加载，但是仅在第一次加载时运行一次，然后运行结果被缓存，之后在加载就从缓存中读取结果。

###### 模块定义/模块标识

~~~javascript
//dataBase.js文件
module.export = {
	"saveData":saveData,
    "data":"data"
}
//此模块下的函数
function saveData(){
    console.log(11);
}
~~~

###### 模块引入

~~~javascript
const dataBase = require("./dataBase");//引入模块，并将生成对象指向dataBase
dataBase.saveData();//调用该模块下的方法
~~~

#### node 运行

* 使用文件下git bash运行命令`node index`即node下运行index.js文件
* 结束node进程，再次运行node前需要结束当前进程，否则报错端口被占用
  * 使用命令`tskill node`  
  * Ctrl+C结束当前进程

###### nodemon

node自动重启工具，代替node命令执行。当文件修改之后无需停掉当前服务重新开启，它会自动重启。

* 安装 `npm install -g nodemon`
* 运行  `nodemon index`采用nodemon运行index

## node核心模块

使用之前需要引入

~~~javascript
const http = require('http'),
	  fs = require('fs'),
	  path = require('path'),
	  router = require('./router');
//自定义模块添加路径，查找更快
~~~

#### HTTP

* server类
  * server.on()  监听服务下的事件，如close,connect,connection,request
  * server.listen(8080)  设置端口
  * server.close([callback])

* response类

  * response.end([data],encoding,[,callback])  可在end中直接传一个要返回给前端的数据。

    这个数据可用文件系统fs.readFile(..)来获取到。服务开启后必须有response.end();

  * response.write(chunk[,encoding]\[,callback])  参数一为字符串，发送给前端的字符串

~~~javascript
const http = requier('http);
http.createServer((req,resp)=>{
	//解决中文乱码
	resp.writeHead(200, {'Content-Type': 'text/html; charset=utf-8'});
    //请求url，从中获取到请求文件是什么。req.url = "/favicon.ico"谷歌浏览器默认的请求网页图标
    console.log(req.url);
    res.write();//返回给前端数据
    resp.end();//结束服务，里面可以传一个数据
}).listen(8080);//监听端口
~~~

#### fs文件系统

~~~javascript
//读取资源
fs.readFile([path],(err,data)=>{
    if(err) throw err;
    else fs.write(data);
})
~~~

* 读取文件

~~~javascript
const fs = require('fs);
fs.readFile("./static/index.html",(err,data)=>{
	if(err) throw err;
    // 只有一个数据返回时可直接在end()里面传递。类似于resp.write();
    else resp.end(data);
})
~~~

* 写文件 向文件夹中写文件进去，文件存在则会替换

  fs.writeFile(file, data[, options], callback)

~~~javascript
const data = new Uint8Array(Buffer.from('Hello Node.js'));
//将data数据写入到message.txt中
fs.writeFile('message.txt', data, (err) => {
  if (err) throw err;
  console.log('The file has been saved!');
});
~~~

#### path

常用方法：

* path.resolve([...paths])  合并路径，直接采用字符串拼接不好

  `/`开头表示绝对路径，`./`表示当前路径,`../`表示上一级

  ~~~~javascript
  path.resolve("/static","index")// static/index
  path.resolve("/static","/mac","dev");// /mac/dev
  path.resolve("/foo/bar","./dev");// foo/bar/dev
  ~~~~

* path.join([paths])

  ~~~javascript
path.join(__dirname,"../public/list.html");
  //从当前目录返回上一级目录，找Public下的list.html
  ~~~
  
* path.format(pathObject)  根据pathOject对象返回路径字符串

  pathObject = {

  * dir 提供dir忽略root
  * root
  * base 提供base忽略name与ext
  * name
  * ext

  }

* path.parse(path)根据path返回pathObject对象

  ~~~javascript
  path.parse('C:\\path\\dir\\file.txt');
  // 返回:
  // { root: 'C:\\',
  //   dir: 'C:\\path\\dir',
  //   base: 'file.txt',
  //   ext: '.txt',
  //   name: 'file' }
  ~~~

## node.js中间件

中间件需要使用npm安装之后再require，而核心模块只需要require就可以使用



## Express框架

基于node平台，快速，开放，极简的Web开发框架

#### 安装

* 全局安装` npm install express -g`
* `mkdir myapp`新建文件夹，并cd到给文件夹
* `npm init -y`初始化项目的package.json文件
* `npm install express --save`   在myapp文件下安装express并保存到依赖列表

#### hello world

~~~javascript
//myapp下的main.js文件   运行：node main
const express = require('express');
const app = express();

app.get('/',(req,resp)=>{
	res.send("hello world");
})

app.listen(3000,()=>{
    console.log("app demo 已经监听在3000端口")
})
~~~

#### express 应用生成器

通过应用生成器工具 `express-generator` 可以快速创建一个应用的骨架。

* `npm install express-generator -g `全局安装生成器工具

* `express -h`查看命令行参数

* 使用express创建应用`express --viw=ejs exApp`其中参数ejs是一种模板引擎

* `cd exApp`进入项目

* `npm install`安装依赖，express创建的应用都有一些需要的依赖，如http等

* `port=8090 npm start`启动项目，端口参数可选，不设置`npm start`的话默认为3000端口

  ~~~javascript
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
  ~~~

#### express框架运用

###### 路由

确定应用程序如何响应客户机的特定端点请求，该请求可以是一个路径，也可以是一个http请求方法

* app.METHOD(PATH,HANDLER)

~~~javascript
//使用规则：app.METHOD(PATH,HANDLER)
app.get('/', function (req, res) {
  res.send('Hello World!');
    //res.end()  res.json()  res.render()   res.send()  res.sendFile()
})
app.get('/', function (req, res) {},function(){})
app.get('/', [fun1,fun2,fun3])
~~~

* app.route()

  为路由路创建处理程序

  ~~~javascript
  app.route('/book')
    .get(function (req, res) {
      res.send('Get a random book')
    })
    .post(function (req, res) {
      res.send('Add a book')
    })
  ~~~

* express.Router()

  定义一个路由模块，在app.use()中使用

  ~~~javascript
  //定义 index.js
  var express = require('express);
  var router = express.router();
  router.use(...);
  router.get("/",function(req,resp){
      res.send("hello");
  })
  //暴露此模块
  module.expres = router;
  ~~~

  ~~~javascript
  //加载路由模块到app应用
  var app = express();
  var indexRouter = require("./index");
  
  app.use("/",indexRouter)
  ~~~

###### 中间件

中间件函数能够访问请求对象，相应对象，以及应用程序的请求/响应循环中的下一个中间件函数。下一个中间件函数使用next()的变量来表示。

中间件的使用顺序会影响页面渲染效果

中间件有应用层中间件，路由层中间件，错误处理中间件，内置中间件，第三方中间件。

~~~javascript
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
~~~

###### 静态资源

提供图片，js，HTML，css等静态资源，使用Express 中的 `express.static` 内置中间件函数

~~~javascript
// 处理静态文件的中间件（静态资源从public中获取)
// 此处访问静态资源的时候需要将资源的文件后缀带上，不然应该会识别成路径进行路由分配
app.use(express.static(path.join(__dirname, 'public')));

// 此静态资源可通过带有/routes前缀来进行访问
app.use('/routes',express.static(path.join(__dirname, 'routes')));
~~~

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

~~~html
<!--表单内容-->
<form action="/file_upload" method="post" enctype="multipart/form-data">
    <p><input type="file" name="image" size="50"></p>
    <input type="submit" value="上传文件" name="">
</form>
~~~

#### 常用API

###### express()

* express.json()  express内置中间件，处理json格式数据的中间件

* express.static(root)  root参数指定为静态资源提供服务的根目录

  ~~~javascript
  //设置静态资源的获取在当前目录找过去的public文件夹下
  app.use(express.static(path.join(__dirname, 'public')));
  ~~~

* express.router()  创建新的router对象

* express.urlencoded() 解析url的中间件

###### application

* app.all(path,callback[,callback])  类似app.METHOD()方法，但它匹配所有http请求

* app.lesten(port) 将连接绑定并监听到特定端口

* app.METHOD()  路由HTTP请求，其中method是请求http的方法，如get，post，put等

* app.get(path,callback[,callback]) 使用指定的回调函数将HTTP GET请求路由到指定的路径

* app.post(path,callback[,callback..])使用指定的回调函数将HTTP POST请求路由到指定的路径

* app.put(path,..)使用指定的回调函数将HTTP PUT请求路由到指定的路径

* app.render() 返回根据回调函数渲染的HTML，类似于resp.render(),但它不能将结果返回给客户端

* app.route() 返回一个路由实例

* app.set(name,value)  name值可以是配置服务器的行为的特殊名称值，如views,视图文件的目录

  ~~~javascript
  app.set('views', path.join(__dirname, 'views'));
  // 设置模板引擎为ejs
  app.set('view engine', 'ejs');
  ~~~

* app.use([path,]callback) 指定一个或多个中间件到指定路径，当请求路径匹配时将执行中间件函数，未指定路径时将响应所有路径请求。

* app.param() 向路由参数添加回调触发器

###### request	

* req.url  获取请求路径，根据路径匹配对应路由

###### response

* resp.end([data],[,encoding])  结束响应进程

* resp.download() 

* resp.get()

* resp.json()

* resp.render() 渲染模板HTML并将结果发送到客户端

  ~~~javascript
  /* GET home page. */
  router.get('/', function(req, resp, next) {
  	// 此处的index是view下的index.ejs文件，在app.js中设置过此路径
  	//app.set('views', path.join(__dirname, 'views'));
  	// 渲染index模板引擎，并且将渲染结果返回给前端。区别于之前的前端渲染方法
    resp.render('index', { title: 'nihao' });
  });
  ~~~

* resp.send() 传送http响应内容，可以是一个字符串，对象，数组

* resp.sendFile(path,[,callback])  除非在选项对象中设置了根选项，否则path必须是文件的绝对路径。

  ~~~javascript
  let filePath = path.join(__dirname,'../public/list.html');
  resp.sendFile(filePath);
  ~~~

* resp.status(code) 设置http状态的处理

  ~~~javascript
  res.status(400).send('Bad Request');
  ~~~

###### router

* router.all()
* router.METHOD()
* router.param()
* router.route()
* router.use()

## ejs模板引擎

#### 用法

* 输出数据到模板

~~~ejs
<%=
	<h2>user</h2>
%>
~~~

* <% 脚本标签，用于控制，无输出，其中可以写语句

~~~ejs
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
~~~

* 输出非转义数据到模板

~~~
<%-
	<h2>user</h2>
%>
~~~

* 自定义分隔符

  ~~~ejs
  // 单个模板文件
  ejs.render('<?= users.join(" | "); ?>', {users: users},{delimiter: '?'});
  
  // 全局
  ejs.delimiter = '$';
  ejs.render('<$= users.join(" | "); $>', {users: users});
  ~~~

* include 将模板路径中的模板片段包含进来

  ~~~ejs
  <ul>
    <% users.forEach(function(user){ %>
      <%- include('user/show', {user: user}); %>
    <% }); %>
  </ul>
  ~~~



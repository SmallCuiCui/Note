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



读取文件

~~~javascript
const fs = require('fs);
fs.readFile("./static/index.html",(err,data)=>{
	if(err) throw err;
    // 只有一个数据返回时可直接在end()里面传递。类似于resp.write();
    else resp.end(data);
})
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

## Express框架

基于node平台，快速，开放，极简的Web开发框架

#### 安装

* 全局安装` npm install express -g`
* `mkdir myapp`新建文件夹，并cd到给文件夹
* `npm init -y`初始化项目的package.json文件
* `npm install express --save`   在myapp文件下安装express并保存到依赖列表

#### hello world

~~~javascript
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
  
  //项目执行过程
  //执行start命令->本质执行www文件 =>www文件创建了服务并监听 =>前端传来请求 =>从www到引入app模块 =>app模块引入了很多依赖的模块、使用中间件、配置等 =>根据请求url进入对应路由 =>routers下的对应路由模块 =>路由中根据url返回对应资源
  ~~~



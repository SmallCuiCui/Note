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

  * *如果是使用nvm进行node的版本控制的话，node与npm的路径在nvm的安装路径下的system文件夹下

  ~~~js
  "node_command": "G:\\app\\node.js\\install\\node.exe",
  //  "node_command": "D:\\ProgramFiles\\nvm\\system\\node.exe",
  
  "npm_command": "G:\\app\\node.js\\install\\npm.cmd"
  // "npm_command": "D:\\ProgramFiles\\nvm\\system\\npm.cmd",
  ~~~

* 配置Nodejs.sublime-build文件

  Preferences-->Browse Package ->Package文件夹->Nodejs文件夹->Nodejs.sublime-build文件

  修改cmd 和 windows 下的cmd 以及encoding

  ~~~js
  "cmd": ["G:\\app\\node.js\\install\\node.exe", "$file"],
  // ["D:\\ProgramFiles\\nvm\\system\\node.exe", "$file"],
  ....
  "encoding": "utf8",
  .....
  "windows":
      {
          "cmd": ["G:\\app\\node.js\\install\\node.exe","$file"]
          // "cmd": ["D:\\ProgramFiles\\nvm\\system\\node.exe","$file"]
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
module.exports = {
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
  * 使用命令`tskill node`  会结束所有node打开的进程服务                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
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

## ejs模板引擎

#### 用法

在app.js中指定模板的文件夹位置，调用render方法时将自动到该文件夹下找模板进行渲染，然后返回给前端

~~~js
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
~~~

~~~
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});
~~~

~~~ejs
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>

~~~



#### 语法

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



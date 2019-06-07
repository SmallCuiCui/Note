### node相关

#### nvm管理node

##### nvm

node版本管理器：安装管理node，用于多个node版本的切换。

##### 下载安装

github搜索nvm，下载安装，注意路径中不要出现中文及空格

##### 常用命令

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

##### 模块化

* commanjs模块规范主要分为：模块定义，模块标识，模块引用
* 一个文件就是一个模块，拥有单独的作用域
* 普通方式定义的函数，参数，对象都属于该模块内
* 通过exports与module.exports来暴露模块中的内容
* 通过require加载该模块，多个模块同步加载。加载时如果为绝对路径则直接根据路径加载，最快，不是路径则到node_modules文件下遍历查找。

* 代码运行在该模块内，不会污染全局，模块可以多次加载，但是仅在第一次加载时运行一次，然后运行结果被缓存，之后在加载就从缓存中读取结果。

##### 模块定义/模块标识

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

##### 模块引入

~~~javascript
const dataBase = require("./dataBase");//引入模块，并将生成对象指向dataBase
dataBase.saveData();//调用该模块下的方法
~~~

#### node 运行

* 使用文件下git bash运行命令`node index`即node下运行index.js文件
* 结束node进程，再次运行node前需要结束当前进程，否则报错端口被占用
  * 使用命令`tskill node`  
  * Ctrl+C结束当前进程

### node核心模块

使用之前需要引入

~~~javascript
const http = require('http'),
	  fs = require('fs'),
	  path = require('path'),
	  router = require('./router');
//自定义模块添加路径，查找更快
~~~

#### HTTP

#### fs文件系统

#### path

，常用方法：

* path.resolve([...paths])  合并路径，采用字符串拼接不好

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

  


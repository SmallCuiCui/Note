#### nvm管理node

##### nvm

安装管理node，用于多个node版本的切换。

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


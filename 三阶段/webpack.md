# webpack

全局安装：

`cnpm install webpack -g`

`cnpm install webpack-cli -g`

项目中使用的时候还需要局部安装。

默认入口文件： src/index.js

默认出口文件： dist/main.js

打包： webpack

指定模式打包：webpack --mode=development -W  也是无配置打包,-w的作用是当文件发生变化时

打包模式：

1. development  我压缩
2. production  压缩

配置文件：webpack.config.js  写在项目的根目录下

解析或打包除了js的文件，都要安装特定的 `loading`

* `.css` 文件： 安装`style-loader`  `css loader`进行config配置，配置时有顺序
* 图片文件：`url-loader`  `file-loader`  前置依赖后者，使用后者

插件：

`html-webpack-plugin`  局部安装。npm run build进行打包的原理。不需要自己引入文件

`webpack-dev-server`   需要全局安装以及局部安装。

babel配置：ES6转ES5

安装： `babel-loader`  `@babel/core`    `@babel/preset-env`



搭建react环境：

安装  `cnpm install react react-dom @babel/preset-react`



国际化，删除。

react-loadable  按需加载






















































































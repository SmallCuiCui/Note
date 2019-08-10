# webpack

JavaScript程序的静态模块打包器，它会递归创建关系依赖图。。。

### 安装

#### 全局安装：

`cnpm install webpack -g`

`cnpm install webpack-cli -g`

#### 局部安装

新建项目文件夹(或直接在项目下)， -D模式局部安装上面两个插件。

### 打包

默认打包： webpack  

指定模式打包：webpack --mode=development -W  也是无配置打包,-w的作用是当文件发生变化时

### 核心概念
以下均是在配置项目根目录下新建的`webpack.config.js`文件。注意名字不能改

#### 入口

绘制关系图的起点。默认入口文件： src/index.js

##### 单个入口的写法

直接是一个路径的字符串

~~~js
module.exports = {
  entry: './src/index.js'
};
~~~

##### 对象语法

~~~~js
module.exports = {
  entry: {
    app: './src/app.js',//应用程序
    vendors: './src/vendors.js'  // 第三方库
  }
};
~~~~

##### 多页面应用程序

webpack会构建多个独立的关系图

~~~js
module.exports = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
~~~

#### 出口

配置最低要求，是一个对象，设置两个值：

* filename  输出文件的文件名
* path  输出文件的路径

入口文件可有多个，但是出口只有一个。默认出口文件： dist/main.js

~~~js
module.exports = {
  output: {
    filename: 'bundle.js',
    // filename: '[name].js', // 多个入口时的写法，对应上面最后输出为pageOne.js，pageTwo.js...
    path: path: __dirname + '/dist'
  }
};
~~~

#### 模式

##### 用法

1. 配置项中添加mode字段。

   ```js
   module.exports = {
     mode: 'production'
   };
   ```

2. 打包时指定 webpack --mode=production

部分包只需要在开发的过程中使用，产品中不需要，比如webpack,eslint等，因此需要指定模式，再打包时可进行模式的选择。配置中设置mode字段

##### development

* 会将process.evn.CODE.ENV设置为development
* 没有压缩文件

##### production 

* 会将process.evn.CODE.ENV设置为production
* 会压缩文件

#### loader

解析或打包除了js以外的文件，都需要安装特定的loader

##### 配置

~~~
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader',css-loader'] },
      { test: /\.(png|jpg|gif)$/, use: 'url-loader' }
    ]
  }
};
~~~

##### 常用loader

* css: 安装`style-loader`  `css loader`进行config配置，配置时有顺序
* 编译sass：

* 图片文件：`url-loader`  `file-loader`  前置依赖后者，使用后者
* 解析.vue文件：vue-loader,注意还需要用到一个插件VueLoaderPlugin
* 解析.ts文件：ts-loader

#### 插件





配置文件：webpack.config.js  写在项目的根目录下

解析或打包除了js的文件，都要安装特定的 `loader`

插件：

`html-webpack-plugin`  局部安装。npm run build进行打包的原理。不需要自己引入文件

`webpack-dev-server`   需要全局安装以及局部安装。

babel配置：ES6转ES5

安装： `babel-loader`  `@babel/core`    `@babel/preset-env`



### 搭建react环境

安装  `cnpm install react react-dom @babel/preset-react`



国际化，删除。

react-loadable  按需加载





















































































